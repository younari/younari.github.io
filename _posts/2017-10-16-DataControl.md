---
layout: post
title: "Data Control"
author: "younari"
---

# 서버와의 데이터 연동시 Data Control 
- **DB, 서버에서 전달 받은 데이터를 바로 사용하지 않고 Struct의 형태로 데이터 모델을 만들어서 사용한다.**

# 데이터모델을 구성하는 이유

# Sample Code v0.1
- User Model Structure 만들기
- ID,PWD,Email,Birthday,Gender로 구성된 User Data를 서버로부터 Dictionary 형태로 받았을 경우 해당 Dictionary를 UserModel Struct 형태로 변환해서 갖고 있도록 작업한다.
- 이후 앱 내에서 User Data를 활용해야 할 경우, UserModel의 인스턴스를 만들어 Dot 문법으로 접근하여 쉽게 사용할 수 있을 것이다.

{% highlight swift %}

import Foundation
struct UserModel {

    enum Gender: Int {
        case women = 1
        case men = 2
    }
    
    var userID: String
    var userPWD: String
    var email: String
    var birthday: String?
    var gender: Gender?
    
    init?(userDic:[String:Any]) {
        
        // Required Properties
        guard let userID = userDic["userID"] as? String else { return nil }
        self.userID = userID
        
        guard let userPWD = userDic["userPWD"] as? String else { return nil }
        self.userPWD = userPWD
        
        guard let email = userDic["email"] as? String else { return nil }
        self.email = email
        
        
        // Optional Properties
        self.birthday = userDic["birthday"] as? String
        
        if let gender = userDic["gender"] as? Int, (gender == 1 || gender == 2)
        {
            self.gender = Gender(rawValue: gender)
        }
        
    }
    
}

{% endhighlight %}

# Sample Code v0.2
- **전시 정보를 담고 있는 Struct 만들어보기**
- imageURL: String, eventName: String, eventPlace: String, startDate: Date, endDate: Date


{% highlight swift %}

import Foundation
struct EventData {
    
    var imageURL: String
    var eventName: String
    var eventPlace: String
    var startDate: Date
    var endDate: Date
    
    init?(dataDic: [String:Any]) {
        guard let imageURL = dataDic["imageURL"] as? String else { return nil }
        self.imageURL = imageURL
        
        guard let eventName = dataDic["eventName"] as? String else { return nil }
        self.eventName = eventName
        
        guard let eventPlace = dataDic["eventPlace"] as? String else { return nil }
        self.eventPlace = eventPlace
        
        // Date 처리
        let formatter = DateFormatter()
        formatter.dateFormat = "yyyy-mm-dd"
        
        guard let startDateStr = dataDic["startDate"] as? String else { return nil }
        guard let startDate = formatter.date(from: startDateStr) else { return nil }
        self.startDate = startDate
        
        guard let endDateStr = dataDic["endDate"] as? String else { return nil }
        guard let endDate = formatter.date(from: endDateStr) else { return nil }
        self.endDate = endDate
    }
}

{% endhighlight %}

## 위 코드에서 Date 만드는 법 뜯어보기
- DateFormatter의 인스턴스 생성
- DateFormatter의 dateFormat 지정
- String을 통한 date 지정 DateFormatter.date(from: String)

{% highlight swift %}

let formatter = DateFormatter()
formatter.dateFormat = "yyyy-mm-dd"

let startDateStr = dataDic["startDate"] as? String else { return nil }
let startDate = formatter.date(from: startDateStr) else { return nil }
self.startDate = startDate

{% endhighlight %}