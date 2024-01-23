# Final Project
### Tool: OpenFrameworks, Programming Language: C++
![Screenshot of my Xcode](https://github.com/HanHsunShih/Coding-Two-Submission/blob/main/%E6%88%AA%E5%9C%96%202023-03-14%2023.16.32.png)
![Screenshot of my Xcode](https://github.com/HanHsunShih/Coding-Two-Submission/blob/main/%E6%88%AA%E5%9C%96%202023-03-14%2023.19.51.png)
## concept
This project utilizes openframworks to generate particles upon the user pressing the space bar on the keyboard. The particles continue to generate as long as the space bar is held on and change colour with each key press. Over time, the particles transition in colour and fade away while also floating in the "z" direction due to the force settings. The perspective of particle system can be adjusted using the mouse, with zooming in and out possible by scrolling. The aim is to create the interaction fireworks effect that can be enjoyed at close range. The effect is designed to provide a virtually stunning scene that immerses the users within the sparkling particles. A force acting on the particles slightly changes the direction of their fall, adding to the user's sense of involvement. When view from above, the user feels as though they are among the fireworks in the sky, or view from bottom, they feel as particles falling toward them.
## Youtube Link
https://youtu.be/LtVutgS3GdI

## Code
### main.cpp
```C++
#include "ofMain.h"
#include "ofApp.h"

//========================================================================
int main( ){
	ofSetupOpenGL(1024,768,OF_WINDOW);			// <-------- setup the GL context

	// this kicks off the running of my app
	// can be OF_WINDOW or OF_FULLSCREEN
	// pass in width and height too:
	ofRunApp(new ofApp());

}
```
### ofApp.cpp
```C++
#include "ofApp.h"

//--------------------------------------------------------------
void ofApp::setup(){
    ofSetBackgroundColor(0, 0, 0);
    hue = 0;
    satuation = 200;
    brightness = 255;
    maxParticles = 2000;
    ofEnableLighting();
    ofEnableDepthTest();
    light.setPosition(0, -600, 300);
    
    ofSetSphereResolution(4);
}

//--------------------------------------------------------------
void ofApp::update(){
    if(particles.size() > maxParticles){
        particles.erase(particles.begin());
    }
    for (int i=0; i<particles.size(); i++){
        particles[i].update(); //呼叫particles向量中第i個元素的update()成員函數
    }
}

//--------------------------------------------------------------
void ofApp::draw(){
    cam.begin();
    ofRotateDeg(180, 0, 0, 1); //translate the camera
    light.enable(); //switch on the light
    ofDrawAxis(400);
    // look through all of the particle object in the particles vector instruct each one to draw itself
    for (int i=0; i<particles.size(); i++){
        particles[i].draw(); //呼叫particles向量中第i個元素的update()成員函數
    }
    light.disable(); //switch the light off
    cam.end();
}

//--------------------------------------------------------------
void ofApp::keyPressed(int key){
    switch (key){
        case 'f':
        case 'F':
            ofToggleFullscreen();
            break;
        case' ':
            int numParticles = 200;
            for (int i = 0; i < numParticles; i++){
                particle newParticle(0, 0, 0, hue, satuation, brightness);
                particles.push_back(newParticle);
            }
            break;
    }
}

//--------------------------------------------------------------
void ofApp::keyReleased(int key){
    hue = ofRandom(255);
}

//--------------------------------------------------------------
void ofApp::mouseMoved(int x, int y ){

}

//--------------------------------------------------------------
void ofApp::mouseDragged(int x, int y, int button){
    //particle newParticle(mouseX, mouseY, hue);
    //particles.push_back(newParticle); //將新創的newParticle放到vector容器的尾端
    
}

//--------------------------------------------------------------
void ofApp::mousePressed(int x, int y, int button){
    hue = ofRandom(255); //put random hue to each button press
}

//--------------------------------------------------------------
void ofApp::mouseReleased(int x, int y, int button){

}

//--------------------------------------------------------------
void ofApp::mouseEntered(int x, int y){

}

//--------------------------------------------------------------
void ofApp::mouseExited(int x, int y){

}

//--------------------------------------------------------------
void ofApp::windowResized(int w, int h){

}

//--------------------------------------------------------------
void ofApp::gotMessage(ofMessage msg){

}

//--------------------------------------------------------------
void ofApp::dragEvent(ofDragInfo dragInfo){ 

}

//--------------------------------------------------------------

particle::particle(int x, int y, int z, int hue, int satuation, int brightness){
    position = glm::vec3(x, y, z);
    position.z = 0;
    force = glm::vec3(0, 0, 0.02);
    direction = glm::vec3(ofRandom(-2.0, 2.0), ofRandom(-2.0, 2.0), ofRandom(-2.0, 2.0));
    size = ofRandom(2, 5);
    color.setHsb(hue, satuation, brightness, 150); //last parameter: transparency
}

//--------------------------------------------------------------

particle::~particle(){
    //destructor
}

//--------------------------------------------------------------

void particle::update(){
    position += direction;
    if(size > 1){
        size -= 0.007;
    }/*else{
        size = 0;
    }*/
    direction += force;
    float brightness = color.getBrightness();
    float myHue = color.getHue();
    
    if (brightness > 10){
        color.setBrightness( brightness -= 0.001);
        color.setHue( myHue -= 1);
    }
}

//--------------------------------------------------------------

void particle::draw(){
    ofSetColor(color);
    ofDrawCircle(position, size);
    
    ofPushMatrix();
        ofTranslate(position);
        ofSetColor(color);
        ofDrawSphere(size);
        ofPopMatrix();
}
```
### ofApp.h
```C++
#pragma once
#include "ofMain.h"

class particle{
public:
    ofColor color;
    float size;
    glm::vec3 force, position, direction;
    
    void update();
    void draw();
    
    particle(int x, int y, int z, int hue, int satuation, int brightness); //constructor
    ~particle(); //destructor
};

class ofApp : public ofBaseApp{

	public:
		void setup();
		void update();
		void draw();

		void keyPressed(int key);
		void keyReleased(int key);
		void mouseMoved(int x, int y );
		void mouseDragged(int x, int y, int button);
		void mousePressed(int x, int y, int button);
		void mouseReleased(int x, int y, int button);
		void mouseEntered(int x, int y);
		void mouseExited(int x, int y);
		void windowResized(int w, int h);
		void dragEvent(ofDragInfo dragInfo);
		void gotMessage(ofMessage msg);
		
    vector<particle> particles; //定義一個名為particles的vector容器，其中存放的元素類型是particle
    int hue;
    int satuation;
    int brightness;
    //這兩行與particle本身無關，而跟整個應用程式有關，所以不寫在class particle裡面
    ofEasyCam cam;
    ofLight light;
    int maxParticles;
};
```
