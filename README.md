# ventilator-2
import java.awt.Rectangle;

import hypermedia.video.*;

OpenCV opencv;

Serial arduinoPort;

import processing.serial.*;  

void setup() {

    size( 320, 240 );
    arduinoPort = new Serial(this, "COM11", 2560); //change this to whichever port your arduino is on
    opencv = new OpenCV(this);
    opencv.capture( width, height );
    opencv.cascade( "C:\\Program Files (x86)\\OpenCV\\data\\haarcascades\\haarcascade_frontalface_alt.xml" );   // change this to where your frontal face xml is located
}

void draw() {
    
    opencv.read();
    image( opencv.image(), 0, 0 );
    
    // detect anything ressembling a FRONTALFACE
    Rectangle[] faces = opencv.detect();
    
    // draw detected face area(s)
    noFill();
    stroke(255,0,0);
    for( int i=0; i<faces.length; i++ ) {
        rect( faces[i].x, faces[i].y, faces[i].width, faces[i].height ); 
    
    if(faces[i].x  < 107){
     println("Left"); 
     arduinoPort.write("d");
   }
     
   if(faces[i].x + faces[i].width > 213){
    println("Right"); 
    arduinoPort.write("a");
   }
  
  if(faces[i].y + faces[i].height > 160){
    println("Bottom"); 
    arduinoPort.write("s");
   }

  if(faces[i].y < 80){
   println("Top"); 
   arduinoPort.write("w");
  }

  }
   
   line(0, 80, 320, 80);
   line(0, 160, 320, 160);
   line(107, 240, 107, 0);
   line(213, 0, 213, 240);  
   
}

