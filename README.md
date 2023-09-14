#just arduino_blutooth
rx,tx 반대로 연결 **
참조 : https://m.blog.naver.com/sisosw/221518483163











# detection-and-Serial
#아두이노 
char cmd[50];  // Create a character array to store the incoming data
#define LED 7

unsigned long previousMillis = 0;
const long interval = 500; 

void setup() {
  // Start serial communication (baud rate: 9600)
  Serial.begin(115200);
  pinMode(LED,OUTPUT);
}

void loop() {
  // When serial communication is transmitted from the computer, read it line by line into the cmd array.
  if (Serial.available()) {
    unsigned long currentMillis = millis();   
    Serial.readBytesUntil('\n', cmd, sizeof(cmd));
    int numDetected = atoi(cmd);
    if (currentMillis - previousMillis >= interval) {
      previousMillis = currentMillis;
      if(numDetected > 0) 
      {
        digitalWrite(LED, HIGH);
      } else {
      digitalWrite(LED, LOW);
      }
    }
    // Clear the cmd array to prepare for the next input
    memset(cmd, 0, sizeof(cmd));
 

  }
}

#파이썬 코드 
import serial
import time

py_serial = serial.Serial(
    
    # Window
    port='COM18',
    
    # 보드 레이트 (통신 속도)
    baudrate=115200,
)

while True:
      
    commend = input('아두이노에게 내릴 명령:')
    
    py_serial.write(commend.encode())
    
    time.sleep(0.1)
    
    if py_serial.readable():
        
        # 들어온 값이 있으면 값을 한 줄 읽음 (BYTE 단위로 받은 상태)
        # BYTE 단위로 받은 response 모습 : b'\xec\x97\x86\xec\x9d\x8c\r\n'
        response = py_serial.readline()
        
        # 디코딩 후, 출력 (가장 끝의 \n을 없애주기위해 슬라이싱 사용)
        print(response[:len(response)-1].decode())

#파이썬 코드 - yolo
if __name__ == '__main__': 밑에     py_serial = serial.Serial("COM18",115200)
그리고
for path, im, im0s, vid_cap, s in dataset:
    # ...

    # Process predictions
    for i, det in enumerate(pred):  # per image
        seen += 1
        if webcam:  # batch_size >= 1
            p, im0, frame = path[i], im0s[i].copy(), dataset.count
            s += f'{i}: '
        else:
            p, im0, frame = path, im0s.copy(), getattr(dataset, 'frame', 0)

        # ... (other code)

        # Send the number of doors detected to Arduino
        num_doors_detected = len(det)
        py_serial.write(str(num_doors_detected).encode())
        time.sleep(0.1)
