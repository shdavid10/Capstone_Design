# MQTT 소스코드   


아두이노에서 시리얼통신을 통해 온도, 심박수의 데이터를 받아 MQTT 브로커에 전송하는 코드 (Python) 

from time import sleep
import paho.mqtt.client as mqtt
import serial
import time

def publish_callback(client,userdata,result):             #create function for callback
    print("data published \n")

# 클라이언트 생성
client = mqtt.Client()

# publish 할때마다 호출되는 콜백함수 publish_callback 함수 설정한다.
client.on_publish = publish_callback

# publish할 주소와 포트번호 연결
client.connect("127.0.0.1", 1883)
client.loop_start()

class SertoDB_OOP():
    def __init__(self):
        self.cur_time = " "
        self.TMP_val = " "
        self.BPM_val = " "
    
    def serial_start(self):
        try:
            self.ser = serial.Serial('com4', 9600)
            self.stop_flag = False
        except:
            print("연결오류 : 연결 상태를 확인 해주세요")
            self.stop_flag = True
            time.sleep(1)
    
    def data_collect(self):
        while not self.stop_flag:
            if self.ser.readable():
                res = self.ser.readline()
                res_decode = res.decode()
                if(res_decode[0:3] == "TMP"):
                    self.TMP_val_raw = res_decode[3:-1]
                    self.TMP_val = float(self.TMP_val_raw)
                elif(res_decode[0:3] == "BPM"):
                    self.BPM_val_raw = res_decode[3:-1]
                    self.BPM_val = float(self.BPM_val_raw)
                    self.cur_time = time.strftime("%y%m%d_%H%M%S")
                    TMP = str(self.TMP_val)
                    BPM = str(self.BPM_val)
                    
                    client.publish("Time", "{\"Time\":"+self.cur_time+"}")
                    client.publish("TMP", "{\"TMP\":"+TMP+"}")
                    client.publish("BPM", "{\"BPM\":"+BPM+"}")

oop = SertoDB_OOP()
oop.serial_start()
oop.data_collect()


