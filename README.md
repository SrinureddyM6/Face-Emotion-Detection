# Face-Emotion-Detection
import tensorflow as tf
from tensorflow import keras
import numpy as np
import cv2

emotion =  ['Anger', 'Disgust', 'Fear', 'Happy', 'Sad', 'Surprise', 'Neutral']
#Loading Trained model(h5 file)
model = keras.models.load_model('C:\\Users\\msrin\\OneDrive\\Desktop\\project1\\project\\facial-expression-recognition-webcam-master\\model_35_91_61.h5')
font = cv2.FONT_HERSHEY_SIMPLEX
cam = cv2.VideoCapture(0)
face_cas = cv2.CascadeClassifier('./cascades/haarcascade_frontalface_default.xml')

while True:
    ret, frame = cam.read()
    
    if ret==True:
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        #gray = cv2.flip(gray,1)
        faces = face_cas.detectMultiScale(gray, 1.3,5)
        
        for (x, y, w, h) in faces:
            face_component = gray[y:y+h, x:x+w]
            fc = cv2.resize(face_component, (48, 48))
            inp = np.reshape(fc,(1,48,48,1)).astype(np.float32)
            inp = inp/255.
            prediction = model.predict_proba(inp)
            em = emotion[np.argmax(prediction)]
            score = np.max(prediction)
            cv2.putText(frame, em+"  "+str(score*100)+'%', (x, y), font, 1, (0, 255, 0), 2)
            cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 0, 255), 2)
        cv2.imshow("image", frame)
        
        if cv2.waitKey(1) == 27:
            break
    else:
        print ('Error')

cam.release()
cv2.destroyAllWindows()



'''The user’s emotions using facial expressions will be 	detected.	These expressions can be derived from the 	live feed via  the system’s camera or any pre-existing 	image available in the memory.
	Emotions possessed by humans can be recognized and 	has a vast scope of study in the computer vision 	industry upon which several types of research have 	already been done.![image](https://github.com/SrinureddyM6/Face-Emotion-Detection/assets/144892349/430a4f3e-6bc9-4a40-9384-78a7897ab300)'''

