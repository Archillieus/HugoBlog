---
date: 2019-04-14T09:43:48+08:00
title: "C++实现一个string类"
draft: false
categories: ["IT技术"]
tags: ["C++"]
series: ["C++从入门到入门"]
toc: false
img: ""
---

尝试自己动手实现一个string类。<!--more-->

## 实现一个string

```c++
#include <utility>
#include <string.h>

class String
{
public:
  String()
    : data_(new char[1])
  {
    *data_ = '\0';
  }

  String(const char* str)
    : data_(new char[strlen(str) + 1])
  {
    strcpy(data_, str);
  }

  String(const String& rhs)
    : data_(new char[rhs.size() + 1])
  {
    strcpy(data_, rhs.c_str());
  }
  /* Delegate constructor in C++11
  String(const String& rhs)
    : String(rhs.data_)
  {
  }
  */

  ~String()
  {
    delete[] data_;
  }

  /* Traditional:
  String& operator=(const String& rhs)
  {
    String tmp(rhs);
    swap(tmp);
    return *this;
  }
  */
  String& operator=(String rhs) // yes, pass-by-value
  {
    swap(rhs);
    return *this;
  }

  // C++ 11
  String(String&& rhs)
    : data_(rhs.data_)
  {
    rhs.data_ = nullptr;
  }

  String& operator=(String&& rhs)
  {
    swap(rhs);
    return *this;
  }

  // Accessors

  size_t size() const
  {
    return strlen(data_);
  }

  const char* c_str() const
  {
    return data_;
  }

  void swap(String& rhs)
  {
    std::swap(data_, rhs.data_);
  }

private:
  char* data_;
};
```