---
title: Python封装的一个简单LOG类
date: 2016-10-10 00:00:18
category: Python语言
---
```` python
#!/usr/bin/env python
# coding=utf-8

import logging
import logging.handlers
import sys


class CLog(object):
    def __init__(self):
        self.m_Logger = logging.getLogger()
        self.m_Logger.setLevel(logging.NOTSET)
        self.m_Formatter = logging.Formatter("[%(asctime)s]|%(levelname)s|%(process)d|%(thread)d|%(filename)s|%(lineno)s|%(funcName)s|%(message)s")

        # 默认先输出到控制台
        tHandler = logging.StreamHandler(sys.stdout)
        tHandler.setFormatter(self.m_Formatter)
        self.m_Logger.addHandler(tHandler)

    def Load(self, strPath, strLogLevel, iLogCount):
        mapLevel = {"debug": logging.DEBUG, "normal": logging.INFO, "warning": logging.WARNING, "error": logging.ERROR}
        iLevel = mapLevel.get(strLogLevel, logging.DEBUG)

        tHandler = logging.handlers.TimedRotatingFileHandler(filename=strPath, when="midnight", backupCount=iLogCount, encoding="utf-8")
        tHandler.setFormatter(self.m_Formatter)
        tHandler.setLevel(iLevel)

        self.m_Logger.addHandler(tHandler)

    @property
    def Debug(self):
        return self.m_Logger.debug

    @property
    def Normal(self):
        return self.m_Logger.info

    @property
    def Warning(self):
        return self.m_Logger.warning

    @property
    def Error(self):
        return self.m_Logger.error


gLogger = CLog()

'''
# 使用演示
if __name__ == "__main__":
    gLogger.Load("test.log","debug",0)
    try:
        doSomething()
    except Exception, args:
        gLogger.Error(args, exc_info=True)
'''

````