
# SDK 接入说明

## 示例

```java
/**
 * @author baimao
 * @create 2018/10/16
 */
public class DataQueryProtocolTest {

    // eos api 地址
    private static final String eosHost = "http://preview.unitedata.link/v1";
    // eos 账户
    private static final String account = "muhe";
    // eos 私钥
    private static final String privateKey = "5KfU6VKjtXCH1RNvPd7hZxQdi9DfAqtGdYwdK38WT97DqXc5R9v";
    // 合约地址
    private static final String contractId = "dbdjeoaxdtti";
    // 交易订单 id
    private static final String transactionId = "";
    // 姓名
    private static final String name = "张三";
    // 身份证
    private static final String idNumber = "411524199664278730";

    // 查询参数
    private Map<String, Object> queryParameter;

    @Before
    public void preQuery() {
        queryParameter = new HashMap<>();
        queryParameter.put("name", "lufeng");
        queryParameter.put("idno", "41152419960527843x");
        queryParameter.put("mobile", "18738121039");
        queryParameter.put("dt", "2018-11-05");
        queryParameter.put("statusTime", "2018-11-05 10:00:00");
    }

    // 信用凭证查询
    @Test
    public void creditQuery() {
        DataQueryProtocol protocol = DataQueryClient
            .newProtocol(account, privateKey)
            .setContractUri(eosHost);

        // 计算因子，一个随机数
        final long factor = System.currentTimeMillis();
        // 二要素 md5
        final String key = ProduceHashUtil.twoHash(name, idNumber);
        // 命中键值 md5
        final String verifyKey = ProduceHashUtil.randomHash(key, String.valueOf(factor));
        for(int i=0;i<1;i++) {
            StopWatch watch = StopWatch.
                    createStarted();
            try {
                Object data = protocol.creditQuery(
                        contractId,
                        transactionId,
                        key, verifyKey, true, factor);
                Logger.defaultLogger().info(JsonUtils.toString(data));
            } catch (Exception cause) {
                throw new RuntimeException(cause);
            }
            finally {
                watch.stop();
            }
            System.out.println("[time]  [watcher]  --> "+ watch.getTime(TimeUnit.MILLISECONDS));
        }
    }

    // 一般数据查询
    @Test
    public void query() {
        try {
            Object data = DataQueryClient.newProtocol(account, privateKey)
                    .setContractUri(eosHost)
//                    .setTradeType(TradeType.typeCount, 100)
                    .setTradeType(TradeType.typeTime, 30)
                    .enableAnonymousQuery("18383456845")
                    .query(contractId, transactionId, queryParameter);
            Logger.defaultLogger().info(JsonUtils.toString(data));
        }
        catch (Exception cause){
            cause.printStackTrace();
        }
    }

    @After
    public void postQuery(){
        // 释放占用的资源
        DataQueryClient.dispose();
    }
}
```

## 配置 API

```java

/**
* 设置合约 uri 访问地址
* @param contractUri 合约 uri 地址
* @return 返回当前协议
*/
public DataQueryProtocol setContractUri(String contractUri)

/**
* 设置自定义交易订单 id 生成器
* @param transactionIdentityBuilder 一个交易订单 id 生成器
* @return 返回当前协议对象
*/
private DataQueryProtocol setTransactionIdentityBuilder(
    TransactionIdentityBuilder transactionIdentityBuilder);

/**
* 设置 http[s] 连接超时时间
* @param timeoutHttpConnect 超时时间
* @return 返回当前协议对象
*/
public DataQueryProtocol setTimeoutHttpConnect(int timeoutHttpConnect);

/**
* 设置 http[s] 流数据读取超时时间
* @param timeoutHttpRead 超时时间，单位毫秒
* @return 返回当前协议对象
*/
public DataQueryProtocol setTimeoutHttpRead(int timeoutHttpRead);

/**
* 设置 http[s] 内容编码方式
* @param httpEncoding 内容编码方式名称
* @return 返回当前协议对象
*/
public DataQueryProtocol setHttpEncoding(String httpEncoding);

/**
* 设置自定义日志
* @param logger 日志处理器
* @return 返回当前协议对象
*/
public DataQueryProtocol setLogger(Logger logger);

/**
* 设置混淆数据数量
* @param obfuscateSize 混淆数据数量
* @return 返回当前协议对象
*/
public DataQueryProtocol setObfuscateSize(int obfuscateSize);

/**
* 设置混淆数据字段明恒
* @param obfuscateField 混淆数据字段名称
* @return 返回当前协议对象
*/
@Deprecated
public DataQueryProtocol setObfuscateField(String obfuscateField);

/**
* 设置数据混淆器
* @param dataObfuscator 一个数据混淆器
* @return 返回当前协议对象
*/
public DataQueryProtocol setDataObfuscator(DataObfuscator dataObfuscator);

/**
* 设置基于时间的交易有效天数，或者基于次数的有效交易次数
* @param tradeType 交易方式 {@link TradeType}
* @param transactionQuantity 基于时间的交易有效天数，或者基于次数的有效交易次数
* @return 返回当前协议对象
*/
public DataQueryProtocol setTradeType(byte tradeType, int transactionQuantity);

/**
* 设置最小预处理交易数量
* @param minPreviousTransactionQuantity 最小预处理交易数量
* @return 返回当前协议独享
*/
public DataQueryProtocol setMinPreviousTransactionQuantity(int minPreviousTransactionQuantity);

/**
* 启用匿踪查询
* @param anonymousKeys 一组匿踪查询的 key
* @return 返回当前协议对象
*/
public DataQueryProtocol enableAnonymousQuery(String... anonymousKeys);

/**
* 禁用匿踪查询
* @return 返回当前协议对象
*/
public DataQueryProtocol disableAnonymousQuery();

/**
* 设置一组匿踪查询的 key
* @param anonymousKeys 一组匿踪查询的 key
* @return 返回当前协议对象
*/
private DataQueryProtocol setAnonymousKeys(String... anonymousKeys);

/**
* 启用数据提供方过滤器
* @param filterPredicate 数据提供方过滤条件
* @return 返回当前协议对象
*/
public DataQueryProtocol enableDataProducerFilter(Predicate<DataProducer> filterPredicate);

/**
* 禁用数据提供方过滤器
* @return 返回当前协议对象
*/
public DataQueryProtocol disableDataProducerFilter();

/**
* 是否需要匿踪查询
* @return 返回当前协议匿踪查询状态
*/
public boolean isRequiredAnonymousQuery();

/**
* 获取当前上下文合约账户名称
* @return 返回当前上下文合约账户名称
*/
public String getAccountName();

/**
* 获取当前上下文合约账户私钥
* @return 返回当前上下文合约账户私钥
*/
public String getAccountPrivateKey();

/**
* 获取当前上下文合约 id
* @return 返回当前上下文合约 id
*/
public String getContractId();

/**
* 获取指定的交易 id
* @return 返回指定的交易 id
*/
public String getTransactionId();

/**
* 获取当前上下文合约 uri 访问地址
* @return 返回当前上下文合约 uri 访问地址
*/
public String getContractUri();

/**
* 获取当前上下文交易 id 生产器
* @return 返回当前上下文交易 id 生产器
*/
public TransactionIdentityBuilder getTransactionIdentityBuilder();

/**
* 获取当前上下文数据生产者筛选条件
* @return 当前上下文数据生产者筛选条件
*/
public Predicate<DataProducer> getDataProducerFilter();

/**
* 获取当前上下文 http[s] 连接超时毫秒
* @return 返回当前上下文 http[s] 连接超时毫秒
*/
public int getHttpConnectTimeoutMilli();

/**
* 获取当前上下文 http[s] 数据流读取超时毫秒
* @return 返回当前上下文 http[s] 数据流读取超时毫秒
*/
public int getHttpReadTimeoutMilli();

/**
* 获取当前上下文 http[s] 内容编码方式
* @return 返回当前上下文 http[s] 内容编码方式
*/
public String getHttpEncoding();

/**
* 获取当前上下文 http[s] 请求超时时间
* @return 返回当前上下文 http[s] 请求超时时间
*/
public int getHttpRequestTimeoutMilli();

/**
* 获取当前上下文日志处理器
* @return 返回当前上下文日志处理器
*/
public Logger getLogger();

/**
* 获取当前上下文查询参数
* @return 返回当前上下文查询参数
*/
public Map<String, Object> getQueryParameters();

/**
* 获取当前上下文数据混淆数量
* @return 返回当前上下文数据混淆数量
*/
public int getObfuscateSize();

/**
* 获取当前上下文数据混淆字段名称
* @return 返回当前上下文数据混淆字段名称
*/
public String getObfuscateField();

/**
* 获取当前上下文数据混淆器
* @return 返回当前上下文数据混淆器
*/
public DataObfuscator getDataObfuscator();

/**
* 获取当前上下文最小预处理订单数量
* @return 返回当前上下文最小预处理订单数量
*/
public int getMinPreviousTransactionQuantity();

/**
* 获取当前上下文交易模型
* @return 返回当前上下文交易模型
*/
public byte getTradeType();

/**
* 获取当前上下文同一笔交易订单的交易次数
* @return 返回当前上下文同一笔交易订单的交易次数
*/
public int getTransactionQuantity();

/**
* 获取一组当前协议匿踪查询 key
* @return 返回一组当前协议匿踪查询 key
*/
public String[] getAnonymousKeys();

/**
* 获取当前上下文消息上报服务地址
* @return 返回当前上下文消息上报服务地址
*/
public String getMessageServiceHost();

/**
* 获取当前上下文 token 获取服务地址
* @return 返回当前上下文 token 获取服务地址
*/
public String getTokenServiceHost();

/**
* 按次交易执行查询操作，并获得查询结果
* @param contractId 合约 id
* @param transactionId 交易 id
* @param queryParameters 查询参数
* @return 返回查询结果
*/
public Object query(String contractId,
                String transactionId, Map<String, Object> queryParameters);

    /**
     * 信用查询
     * @param contractId 合约
     * @param transactionId 订单 id
     * @param md5Code 对象 MD5（如：二要素 MD5）
     * @param verifyMd5Code 命中键值 MD5
     * @param continuePreciseQuery 精确查询状态。{@code true} 支持精确查询；否则，不做精确查询
     * @param requestedFactor 查询随机数，命中键值计算因子
     * @return
     */
    public Object creditQuery(
            String contractId,
            String transactionId, String md5Code, String verifyMd5Code, boolean continuePreciseQuery, long requestedFactor);

```
