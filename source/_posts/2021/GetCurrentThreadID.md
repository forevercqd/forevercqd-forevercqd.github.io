---
title: 获取线程ID
catalog: true
date: 2021-08-25 17:55:26
subtitle:
header-img:
tags:
---


#pragma once

#if !defined(_WIN32)
#include <unistd.h>
#endif

#include <cstdlib>
#include <pthread.h>


#if defined(__ANDROID__)
#include <sys/prctl.h>
typedef pid_t pthread_id_t;
#define THREAD_NONE -10000
#elif defined(__IOS__) || defined(__MAC__)
typedef pthread_t pthread_id_t;
#define THREAD_NONE nullptr
#elif defined(__linux__)
#include <sys/syscall.h>
typedef pid_t pthread_id_t;
#define THREAD_NONE -10000
#elif defined(_WIN32)
typedef pid_t pthread_id_t;
#define THREAD_NONE -10000
#endif

static pthread_id_t getCurTid() {
#if defined(__ANDROID__) || defined(__IOS__) || defined(__MAC__)
    return gettid();
#elif defined(__linux__)
    return pthread_self();
#elif defined(_WIN32)
    return pthread_getw32threadid_np(pthread_self());
#else
    return gettid();
#endif

}
