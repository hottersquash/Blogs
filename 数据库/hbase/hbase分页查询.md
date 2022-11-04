#### 1、分页查询

#### 利用pageFilter实现，pageFilter实际上相当于的一个limit的功能

```java
 /**
     *
     * 1、先查询共有多少条，按照条数进行分页
     * 2、在循环中利用rowkey分页（可以添加其他的filter）
     *
     * @param tableName
     * @param batchSize
     * @param lastRowkey 起始rowkey
     */
    public void nextPageByFilter(String tableName,int batchSize,String lastRowkey) throws IOException {
        Connection conn=null;
        Table pira_raw_data =null;
        try {
            conn=HBaseManager.getConnection();
            // 查询总行数
            long allCount = HBaseCountRow.rowCountByCoprocessor(tableName);
            // 页数
            if  (allCount<=0){
                allCount = HBaseCountRow.rowCountByScanFilter(tableName);
            }
            if  (allCount<=0){
                System.out.println("查询到"+tableName + "行数为0");
                return;
            }
            System.out.println("查询到总行数:"+allCount);
            // 总页数
            long allPage = allCount%batchSize==0 ? (allCount/batchSize):(allCount/batchSize+1);
            System.out.println("查询到总页数:"+allPage);
            // 表
            HTable table = (HTable) conn.getTable(TableName.valueOf(tableName));

            for (long pageNumber=0;pageNumber<allPage;pageNumber++) {
                Scan scan = new Scan(Bytes.toBytes(lastRowkey));
                PageFilter filter = new PageFilter(batchSize);
                scan.setFilter(filter);
                ResultScanner rs = pira_raw_data.getScanner(scan);
                for (Result r : rs) {
                    String inputTime = Bytes.toString(r.getValue("info".getBytes(), "inputtime".getBytes()));
                    lastRowkey = Bytes.toString(r.getRow());
                }
                System.out.printf("查询进度：%d/%d\n",pageNumber,allPage);
            }
            System.out.println("查询结束");
        }catch(Exception e) {
            ARE.getLog().error(e);
        }finally {
            try {
                pira_raw_data.close();
                HBaseManager.closeConnection();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

```









#### 2、查询表的总行数

```java
    /**
     * 利用协处理器来统计行数
     * @param tablename
     */
    public static long rowCountByCoprocessor(String tablename){
        long allCount = 0L;
        try {
            Connection conn=HBaseManager.getConnection();
            //提前创建connection和conf
            Admin admin = conn.getAdmin();
            TableName name= TableName.valueOf(tablename);
            // 先disable表，添加协处理器后再enable表
            // admin.disableTable(name);
            HTableDescriptor descriptor = admin.getTableDescriptor(name);
            String coprocessorClass = "org.apache.hadoop.hbase.coprocessor.AggregateImplementation";
            if (! descriptor.hasCoprocessor(coprocessorClass)) {
                System.out.println("服务端未添加<org.apache.hadoop.hbase.coprocessor.AggregateImplementation>协处理器");
                return -1;
                // descriptor.addCoprocessor(coprocessorClass);
            }
            // admin.modifyTable(name, descriptor);
            // admin.enableTable(name);
            Scan scan = new Scan();
            AggregationClient aggregationClient = new AggregationClient(HBaseManager.config);
            allCount = aggregationClient.rowCount(name, new LongColumnInterpreter(), scan);
            System.out.println("RowCount: " + allCount);
        } catch (Throwable e) {
            e.printStackTrace();
        }
        return allCount;
    }

    /**
     * 通过过滤器实现统计行数
     * @param tablename
     * @return
     */
    public static long rowCountByScanFilter(String tablename){
        long rowCount = 0;
        try {
            Connection conn=HBaseManager.getConnection();
            TableName name=TableName.valueOf(tablename);
            //connection为类静态变量
            Table table = conn.getTable(name);
            Scan scan = new Scan();
            //FirstKeyOnlyFilter只会取得每行数据的第一个kv，提高count速度
            scan.setFilter(new FirstKeyOnlyFilter());
            ResultScanner rs = table.getScanner(scan);
            for (Result result : rs) {
                rowCount += result.size();
            }
            System.out.println("RowCount: " + rowCount);
        } catch (Throwable e) {
            e.printStackTrace();
        }
        return rowCount;
    }

```



