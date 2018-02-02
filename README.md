# Face-recognization-with-SQLite-Database
Face recognization using Open cv python with SQLite Database<br>

Hey whats up guys ;) I know that you are very exited for deep learning :D <br>
Details about Face recognization and SQLite Database <br>

Thanks to OpenCV, coding facial recognition is now easier than ever. There are three easy steps to computer coding facial recognition, which are similar to the steps that our brains use for recognizing faces.<br> These steps are:

01:Data Gathering: Gather face data (face images in this case) of the persons you want to identify.<br>
02:Train the Recognizer: Feed that face data and respective names of each face to the recognizer so that it can learn.<br>
03:Recognition: Feed new faces of that people and see if the face recognizer you just trained recognizes them.<br>

First Download SQLite Studio For python: https://sqlitestudio.pl/index.rvt?act=download <br>
After download you may goto download folder and extract the tools and click SQLiteStudio <br>
Then create database and create column of the table and set all attribute whatver you want. <br>

Follow this step now: <br>


import numpy as np<br>
import cv2<br>
import sqlite3<br>
cap = cv2.VideoCapture(0)<br>
detector= cv2.CascadeClassifier('haarcascade_frontalface_default.xml')<br>

def insertOrUpdate(Id,Name):<br>
    conn=sqlite3.connect("FaceBase.db")<br>
    cmd="SELECT * FROM People WHERE ID="+str(Id)<br>
    cursor=conn.execute(cmd)<br>
    isRecordExist=0<br>
    for row in cursor:<br>
        isRecordExist=1<br>
    if(isRecordExist==1):<br>
           cmd="UPDATE people SET Name=' "+str(name)+" ' WHERE ID="+str(Id)<br>
 
    else:<br>
         cmd="INSERT INTO people(ID,Name) Values("+str(Id)+",' "+str(name)+" ' )"<br>
      
    conn.execute(cmd)<br>
    conn.commit()<br>
    conn.close()<br>
    
    
id=raw_input('enter user id')<br>
name=raw_input('enter your name')<br>
insertOrUpdate(id,name)<br>
sampleNum=0;<br>

while(True):<br>
    ret, img = cap.read()<br>
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)<br>
    faces = detector.detectMultiScale(gray, 1.3, 5)<br>
    for (x,y,w,h) in faces:<br>
        sampleNum=sampleNum+1;<br>
        cv2.imwrite("dataSet/User."+str(id)+"."+str(sampleNum)+".jpg",gray[y:y+h,x:x+w])<br>
        cv2.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)<br>
        cv2.waitKey(100);<br>

    cv2.imshow('Face',img)<br>
    cv2.waitKey(1);<br>
    if(sampleNum>20):<br>
        break<br>
    
cap.release()<br>
cv2.destroyAllWindows()<br>




