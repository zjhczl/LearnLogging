## logging

## 基本用法
```
import logging

logging.basicConfig(level=logging.INFO,  # 设置日志级别
                    format='%(asctime)s - %(levelname)s - %(message)s',  # 设置日志格式
                    filename='app.log',  # 设置输出文件（若不设置，默认输出到控制台）
                    filemode='w')  # 文件写入模式

logging.debug("这是 DEBUG 级别的消息")
logging.info("这是 INFO 级别的消息")
logging.warning("这是 WARNING 级别的消息")
logging.error("这是 ERROR 级别的消息")
logging.critical("这是 CRITICAL 级别的消息")

```
## 使用json配置文件
```
import logging
import logging.config

# 定义字典配置
log_config = {
    'version': 1,
    'disable_existing_loggers': False,  # 是否禁用现有的记录器
    'formatters': {
        'simple': {
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        },
        'detailed': {
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s - %(filename)s - %(lineno)d',
        },
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',  # 控制台处理器
            'level': 'DEBUG',  # 处理器的日志级别
            'formatter': 'simple',  # 使用定义的格式化器
        },
        'file': {
            'class': 'logging.FileHandler',  # 文件处理器
            'level': 'INFO',  # 处理器的日志级别
            'formatter': 'detailed',  # 使用定义的格式化器
            'filename': 'app.log',  # 日志文件名
            'mode': 'w',  # 文件写入模式
        },
    },
    'loggers': {
        'my_module': {  # 定义记录器
            'level': 'DEBUG',  # 记录器的日志级别
            'handlers': ['console', 'file'],  # 使用的处理器
            'propagate': False,  # 是否向上传播日志
        },
    },
}

# 应用字典配置
logging.config.dictConfig(log_config)

# 获取记录器
logger = logging.getLogger('my_module')

# 测试日志
logger.debug("这是一个 DEBUG 级别的日志")
logger.info("这是一个 INFO 级别的日志")
logger.warning("这是一个 WARNING 级别的日志")
logger.error("这是一个 ERROR 级别的日志")
logger.critical("这是一个 CRITICAL 级别的日志")

```

## 日志轮转

```
import logging
from logging.handlers import RotatingFileHandler

# 创建日志记录器
logger = logging.getLogger('my_logger')
logger.setLevel(logging.DEBUG)

# 配置 RotatingFileHandler
handler = RotatingFileHandler(
    'app.log',      # 日志文件名
    maxBytes=1024,  # 单个日志文件的最大字节数（例如 1 KB）
    backupCount=5   # 保留最近的 5 个日志文件
)

# 配置日志格式
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)

# 添加处理器到记录器
logger.addHandler(handler)

# 生成一些日志测试
for i in range(100):
    logger.debug(f'日志记录 {i}')

```