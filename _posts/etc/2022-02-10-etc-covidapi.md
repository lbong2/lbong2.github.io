---
layout: post
title: covid-19 api 프로그램
description: >
    covid-19 api 프로그램
hide_last_modified: false
categories:
  - etc
---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8919104066540378"
     crossorigin="anonymous"></script>

# covid-19 api 프로그램

간단한 api 프로그램.  Wikidocs에서 자동 트레이드 프로그램 따라서 만들다가 api만지는거 재밌어서 만들게 됨. 그래서 자동 트레이드 프로그램 코드랑 매우 닮았음..

## 1. covid.py

~~~python
from covidApi import *
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
from PyQt5 import uic
form_class = uic.loadUiType("covid.ui")[0]

class MyWindow(QMainWindow, form_class):

    def __init__(self):
        super().__init__()
        self.setupUi(self)
        self.covidapi = covidApi()
        self.setTableWidgetData()
    def setTableWidgetData(self):
        for j in range(0, 18):
            for i in range(0, 6):
                item = QTableWidgetItem(self.covidapi.data[i][j].text)
                item.setTextAlignment(Qt.AlignVCenter | Qt.AlignRight)
                self.tableWidget.setItem(j, i, item)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    myWindow = MyWindow()
    myWindow.show()
    app.exec_()

~~~

## 2. covidApi.py

~~~python
import requests
from bs4 import BeautifulSoup
import datetime
import json


import sys
from PyQt5.QtWidgets import *
from PyQt5.QAxContainer import *
from PyQt5.QtCore import *


message_list = [
    "gubun",
    "defCnt",
    "incDec",
    "qurRate",
    "isolClearCnt",
    "deathCnt",
]
class covidApi(QAxWidget):
    def __init__(self):
        super().__init__()
        self.now = int(datetime.datetime.now().strftime("%Y%m%d")) - 1
        self.API_Key = "Own your api Key"
        self.URL = "http://openapi.data.go.kr/openapi/service/rest/Covid19/getCovid19SidoInfStateJson?" \
                  "serviceKey={API}" \
                  "&pageNo=1" \
                  "&numOfRows=10" \
                  "&startCreateDt={date}" \
                  "&endCreateDt={date}".format(API=self.API_Key, date=self.now)
        self.data_list = []
        self.api_connect(requests.get(self.URL))

    def api_connect(self, response):
        if response.status_code == 200:
            html = response.text
            self.soup = BeautifulSoup(html, "html.parser")
            self.set_data()

        else:
            print("response code Error!! error code : {}".format(response.status_code))

    def set_data(self):
        self.data = []
        for i in range(0, 6):
            self.data.append(self.soup.select("item {}".format(message_list[i])))
            #for x in data:
             #   tmp = json.dumps(xmltodict.parse(str(x))['item'])
              #  self.data_list.append(tmp)

    def show(self):
        for x in self.data:
            print(x[0].text)



if __name__ == "__main__":
    app = QApplication(sys.argv)
    covidApi = covidApi()
    covidApi.show()

~~~

## result

![그림1](/assets/img/etc/covid.jpg)

다른거 공부해야 하는데 재밌는 것만 찾네 ㅠㅠ
