






#include "stdafx.h"
#include "cv.h"
#include "highgui.h"

//function for face detection 
void detect_and_draw(IplImage* img){
 //initialize
 CvHaarClassifierCascade *cascade=0;
 CvMemStorage *storage=0;

 IplImage* gray=img;

 //baca file xml u/ image detection
 if(!cascade){
  char * file="C:/OpenCV2.1/data/haarcascades/haarcascade_frontalface_alt.xml";
  cascade=(CvHaarClassifierCascade*) cvLoad(file, 0, 0, 0);
  storage=cvCreateMemStorage(0);
 }

 //face detection
 CvSeq* faces=cvHaarDetectObjects(
  gray, 
  cascade,
  storage,
  1.1,
  3,
  CV_HAAR_DO_CANNY_PRUNING,
  cvSize(10, 10));

 int i;

 //detect the face inside the red box that has been found
 for(i=0; i<(faces ? faces->total : 0) ; i++){
  CvRect* r=(CvRect*) cvGetSeqElem(faces, i);
  cvRectangle(img,
   cvPoint(r->x, r->y),
   cvPoint(r->x + r->width, r->y + r->height),
   CV_RGB(255, 0, 0),
   1, 8, 0
   );
 }

 //show the detection image
 cvNamedWindow("success");
 cvShowImage("success", img);

 cvWaitKey(0);
}

int main(array<System::String ^> ^args){
 //loading for a real image
 const char* filename="dank.jpg";
 IplImage* img=cvLoadImage(filename);

 //call function
 detect_and_draw(img);
}
