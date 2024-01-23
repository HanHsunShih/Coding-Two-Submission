# Inheritance
### YouTubeChannel Class (Base Class): 
properties: Name, OwnerName, SubscribersCount, PublishedVideoTitles. 
Methods: Subscribe, Unsubscribe, PublishVideo, and GetInfo.
### CookingYouTubeChannel Class (Derived Class): 
It inherits the properties and methods of the YouTubeChannel class, but it also has its own unique method, Practice.

# Image
![Screenshot of my Xcode](https://github.com/HanHsunShih/Coding-Two-Submission/blob/main/%E6%88%AA%E5%9C%96%202023-03-06%2023.19.28.png)

# Code C++
```C++
//
//  main.cpp
//  inheritance and polymorphism
//
//  Created by 施涵薰 on 06/03/2023.
//

#include <iostream>
#include <list>
using namespace std;

// base class class: 放struc(valuable), function
class YouTubeChannel {
private:
    string Name;
    string OwnerName;
    int SubscribersCount;
    list<string> PublishedVideoTitles;
public:
    YouTubeChannel(string name, string ownerName){
        Name = name;
        OwnerName = ownerName;
        SubscribersCount = 0;
    }
    void GetInfo(){
        cout << "Name: " << Name << endl;
        cout << "OwnerName: " << OwnerName << endl;
        cout << "SubscribersCount: " << SubscribersCount << endl;
        cout << "Videos: " << endl;
        for(string videoTitle : PublishedVideoTitles){
            cout << videoTitle << endl;
        }
    }
    void Subscribe(){
        SubscribersCount++;
    }
    void Unsubscribe(){
        if(SubscribersCount>0){
            SubscribersCount--;
        }
    }
    void PublishVideo(string title){
        PublishedVideoTitles.push_back(title);
    }
};

// derived class
class CookingYouTubeChannel:public YouTubeChannel {
public:
    CookingYouTubeChannel(string name, string ownerName):YouTubeChannel(name, ownerName){
        
    }
    void Practice(){
        cout << "practicing cooking, learning new recipes, experimenting with spices..." << endl;
    }
};

int main() {
    
    CookingYouTubeChannel cookingYtChannel("Amy's Kitchen", "Amy");
    cookingYtChannel.PublishVideo("Apple pie");
    cookingYtChannel.PublishVideo("Carbonara");
    cookingYtChannel.Subscribe();
    cookingYtChannel.Subscribe();
    cookingYtChannel.GetInfo();
    cookingYtChannel.Practice();

    
    system("pause>0");
}
```
