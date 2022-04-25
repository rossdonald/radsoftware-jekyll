---
title: Send SMS with Twilio and .NET Core
# subtitle: >-
#   This is the subtitle for the post.
excerpt: >-
  How to send SMS messages in your dotnet core application using Twilio services.
date: '2022-04-25'
thumb_img_path: images/blog/send-sms.svg
content_img_path: images/blog/send-sms.svg
layout: post
---

Twilio offers an easy way to send SMS from your dotnet application by using their API service. In this article we will walk through the steps to add SMS sending to your .NET core application. The only catch is that phone numbers need to be formatted very specifically (as E164) so I'll provide a solution to that as well.

## Installing Packages

Twilio has a NuGet package that makes using their API simple. Just install the `Twilio` package into your project.

In Visual Studio package manager console:

```
install-package Twilio
```

or at the command prompt:

```
dotnet add package Twilio
```

or use NuGet in Visual Studio.

## Sending a Message

Sending a message is simple using the Twilio classes.

First call `TwilioClient.Init()` and pass in your `Sid` and `AuthToken`. These are like a username and password so they must be protected correctly. You can find your account Sid and Token in the Twilio console.

Then call `MessageResource.CreateAsync()` and pass in `body`, `from`, and `to`

The `from` phone number must be registered in your account as a sending number.

```c#
using Twilio;
using Twilio.Rest.Api.V2010.Account;

public async Task<bool> SendMessageAsync(string toPhone, string messageBody)
{
  TwilioClient.Init(sid, authToken);

  var twilioMessage = await MessageResource.CreateAsync(
    body: messageBody,
    from: new Twilio.Types.PhoneNumber(_settingsSms.From),
    to: new Twilio.Types.PhoneNumber(toPhone)
  );
    
  return true;
}

``` 

## Formatting Phone numbers correctly

Since Twilio is a global service it needs an internationally formatted phone number so it knows which country the message will be sent to. If you don't format the phone numbers in E.164 then you will get errors sending some messages.

> E.164 is the international telephone numbering format  that ensures each phone has a globally unique number. 
>
> E.164 numbers are formatted [+] [country code] [phone number including area code] e.g. +617409999999

You could use regular expressions or write your own validation but there are many rules to internationally formatted phone numbers so it is better to use a package that can handle all of the exceptions and details.

### Formatting API

Twilio has an API that can validate and format phone numbers which you could use.

But that API calls a package which we can call directly and save an API call.

The package is called `libphonenumber`, which is Google's open source phone number handling library.

There is a dotnet port available called `libphonenumber-csharp` which we will use.

### Install libphonenumber-csharp

In Visual Studio package manager console:

```
install-package libphonenumber-csharp
```

or at the command prompt:

```
dotnet add package libphonenumber-csharp
```

or use NuGet in Visual Studio.

### Formatting to international format

When parsing the phone number a default region is defined as a fall-back in case no international code is present. 

For example if most of your phone numbers are from Australia then set the region to `AU`

The `Parse()` method is called, exception handling is recommended as  `NumberParseException` can be thrown on invalid numbers.

Then call `Format()` and specify `PhoneNumberFormat.E164` to get the international format.

Example code:

```c#
const string REGION_DEFAULT = "AU";

PhoneNumberUtil _phoneUtility = PhoneNumberUtil.GetInstance();
PhoneNumber phoneInfo;

phoneInfo = _phoneUtility.Parse(toPhone, REGION_DEFAULT);

// Change to E164 format
string toPhoneE164 = _phoneUtility.Format(phoneInfo, PhoneNumberFormat.E164);
```

## Example with International Formatting

Here is the full example code with formatting. This will prevent errors being raised by international phone numbers in your application.

```c#
using Twilio;
using Twilio.Rest.Api.V2010.Account;

public class TwilioSmsSender
{
  const string REGION_DEFAULT = "AU";
  // Example code only - must be stored securely
  const string sid = "yoursid";
  const string authToken = "yourauthtoken";

  public async Task<bool> SendMessageAsync(string toPhone, string messageBody)
  {
    PhoneNumberUtil _phoneUtility = PhoneNumberUtil.GetInstance();
    PhoneNumber phoneInfo;
    
    phoneInfo = _phoneUtility.Parse(toPhone, REGION_DEFAULT);
    
    // Change to E164 format
    string toPhoneE164 = _phoneUtility.Format(phoneInfo, PhoneNumberFormat.E164);

    TwilioClient.Init(sid, authToken);

    try
    {
      // Send the SMS
      var twilioMessage = await MessageResource.CreateAsync(
        body: messageBody,
        from: new Twilio.Types.PhoneNumber(_settingsSms.From),
        to: new Twilio.Types.PhoneNumber(toPhoneE164)
      );
    }
    catch
    {
      return false;
    }

    return true;
  }
}

```

