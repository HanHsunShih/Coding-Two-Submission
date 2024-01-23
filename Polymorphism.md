# Polymorphism

# Image
![Screenshot of my Xcode](https://github.com/HanHsunShih/Coding-Two-Submission/blob/main/%E6%88%AA%E5%9C%96%202023-03-07%2000.09.07.png)

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
    int SubscribersCount;
    list<string> PublishedVideoTitles;
protected:
    string OwnerName;
    int ContentQuality;
public:
    YouTubeChannel(string name, string ownerName){
        Name = name;
        OwnerName = ownerName;
        SubscribersCount = 0;
        ContentQuality = 0;
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
    void CheckAnalytics(){
        if(ContentQuality < 5)
            cout << Name << " has bad quality content." << endl;
         else
            cout << Name << " has good content." << endl;
        
    }
};

// derived class
class CookingYouTubeChannel:public YouTubeChannel {
public:
    CookingYouTubeChannel(string name, string ownerName):YouTubeChannel(name, ownerName){
        
    }
    void Practice(){
        cout << OwnerName << " practicing cooking, learning new recipes, experimenting with spices..." << endl;
        ContentQuality ++;
    }
};

class SingersYouTubeChannel:public YouTubeChannel {
public:
    SingersYouTubeChannel(string name, string ownerName):YouTubeChannel(name, ownerName){
        
    }
    void Practice(){
        cout << OwnerName << " is taking singing classes, learning new songs, learning how to dance..." << endl;
        ContentQuality ++;
    }
};

int main() {
    
    CookingYouTubeChannel cookingYtChannel("Amy's Kitchen", "Amy");
    /*cookingYtChannel.PublishVideo("Apple pie");
    cookingYtChannel.PublishVideo("Carbonara");
    cookingYtChannel.Subscribe();
    cookingYtChannel.Subscribe();
    cookingYtChannel.GetInfo();
    */
    cookingYtChannel.Practice();
    cookingYtChannel.Practice();
    cookingYtChannel.Practice();
    cookingYtChannel.Practice();
    cookingYtChannel.Practice();
    
    SingersYouTubeChannel singersYtChannel("John sings", "John");
    singersYtChannel.Practice();
    
    YouTubeChannel* yt1 = &cookingYtChannel;
    YouTubeChannel* yt2 = &singersYtChannel;
    
    yt1->CheckAnalytics();
    yt2->CheckAnalytics();
    
    
    system("pause>0");
}

```
