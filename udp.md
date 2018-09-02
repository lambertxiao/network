## UDP协议

* 基本特点

    * 无连接的
    
    * 不保证可靠交付
    
    * 面向报文的，在添加了UDP协议的首部后就直接塞给UP层了
    
    * 没有拥塞控制
    
    * 支持一对一，一对多和多对多的交互通信
    
    * UDP首部开销小
    
* UDP首部格式
    
    * 源端口
    
    * 目的端口
    
    * 长度
    
    * 校验和（检测UDP数据包在传输中是否有错，有错就丢弃）