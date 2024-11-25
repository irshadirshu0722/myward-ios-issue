it's first time i am asking question in stack overflow . I hope you this will help me to solve the issue.

# About project 
- I am using expo and axios for api request 
- use hooks for handling the request 
- using Django as backend 


# Problem Overview
- Api request (all type of request) working properly in Android and Web
- In IOS the POST request working properly But Get request not working 
- Getting this error when request get request from IOS "AxiosError: Network Error"
- I tested the Get request in IOS with other API tester app in IOS then it worked fine


# Expo App some codes

**app.json**
```

{
  "expo": {
    "name": "MyWard",
    "platforms": [
      "ios",
      "android"
    ],
    "scheme": "com.myward",
    "slug": "myward",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/logo/thumbnail.png",
    "userInterfaceStyle": "automatic",
    "ios": {
      "bundleIdentifier": "com.myward",
      "supportsTablet": false,
      "jsEngine": "jsc",
      "infoPlist": {
        "LSApplicationQueriesSchemes": ["whatsapp"]
      },
      "runtimeVersion": {
        "policy": "appVersion"
      }
    },
    "splash": {
      "image": "./assets/images/logo/thumbnail.png",
      "backgroundColor": "#ffffff"
    },
    "jsEngine": "hermes",
    "android": {
      "package": "com.myward",
      "googleServicesFile": "./android/app/google-services.json",
      "runtimeVersion": "1.0.0"
    },
    "web": {
      "bundler": "metro",
      "output": "static",
      "favicon": "./assets/images/logo/thumbnail.png"
    },
    "plugins": [
      "expo-router",
      "expo-localization",
      [
        "expo-build-properties",
        {
          "android": {
            "usesCleartextTraffic": true
          }
        }
      ]
    ],
    "experiments": {
      "typedRoutes": true
    },
    "extra": {
      "router": {
        "origin": false
      },
      "eas": {
        "projectId": "48729d2e-6a7b-4b70-811c-c0b03ad2fd89"
      }
    },
    "owner": "myward",
    "updates": {
      "url": "https://u.expo.dev/48729d2e-6a7b-4b70-811c-c0b03ad2fd89"
    }
  }
}

```
 

**useApiHook**
```


import { useState, useCallback } from "react";
import axios, { AxiosRequestConfig, Method } from "axios";
import { useGlobalContext } from "@/context/GlobalProvider";

interface UseAxiosFetchProps<T> {
  isFetching: boolean;
  fetch: (
    method: Method,
    endpoint: string,
    data?: any,
    token?: string,
    contentType?: ContentType, // Add this parameter
    reloadEffect?: boolean
  ) => Promise<ApiRespond>; // Return Promise
  isFetchedData: boolean;
}

interface ApiRespond {
  ok: boolean;
  data?: any;
  error?: any;
  status: number;
}
type ContentType = "application/json" | "multipart/form-data";
function useApi<T = any>(): UseAxiosFetchProps<T> {
  const [isFetching, setIsFetching] = useState<boolean>(false);
  const [isFetchedData, setIsFetchedData] = useState<boolean>(false);
  const { setIsLoadingOverlay } = useGlobalContext();
  // Fetch function
  const fetch = useCallback(
    async (
      method: Method,
      endpoint: string,
      data?: any,
      token?: string,
      contentType?: ContentType,
      reloadEffect = true
    ): Promise<ApiRespond> => {
      reloadEffect && setIsLoadingOverlay(true);
      setIsFetching(true);

      const config: AxiosRequestConfig = {
        method,
        url: endpoint,
        headers: {
          ...(token ? { Authorization: Bearer ${token} } : {}),
          ...(contentType ? { "Content-Type": contentType } : {}),
        },
        data
      };
      console.log(config);
      let responseData;
      try {
        const response = await axios.request<T>(config);
        responseData = {
          ok: true,
          data: response.data,
          status: response.status,
        };
      } catch (error: any) {
        console.log(error);
        responseData = {
          ok: false,
          status: error.response ? error.response.status : 408,
          error: error.response?.data,
        };
      } finally {
        reloadEffect && setIsLoadingOverlay(false);
        setIsFetching(false);

        if (!isFetchedData) setIsFetchedData(true);
      }
      return responseData;
    },
    []
  );

  return { isFetching, fetch, isFetchedData };
}

export default useApi;


```


**The first GET request i making here**
```

 const verifyUser = useCallback(async () => {
    const token = await _retrieveData("authToken");
    if (token) {
      const response = await fetch(
        "GET",
        API_ENDPOINTS.VERIFY_USER,
        {},
        token,
        "application/json",
        false
      );

      if (response.ok) {
        const data: userDataType = response.data;
        login(
          data.auth_token,
          data.user_preference.language,
          data.user_preference.theme,
          data.profile!,
          data.phone_number
        );
      } else {
        openWelcomeText();
      }
    } else {
      openWelcomeText();
    }
  }, [fetch, login]); // Dependencies: fetch and login

  useEffect(() => {
    verifyUser();
  }, []);



```


# Django backend

**settings.py**

```

Django settings for backend project.

Generated by 'django-admin startproject' using Django 5.0.7.

For more information on this file, see
https://docs.djangoproject.com/en/5.0/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/5.0/ref/settings/
"""
import cloudinary
import os
from pathlib import Path
from decouple import config, Csv

BASE_DIR = Path(__file__).resolve().parent.parent



SECRET_KEY = config('SECRET_KEY')

DEBUG = True

ALLOWED_HOSTS = ['*']


INSTALLED_APPS = [
    'users',
    'profiles',
    'posts',
    'dashboard',
    'subscriptions',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.postgres',
    'corsheaders',
    'rest_framework',
    'cloudinary_storage',
    'cloudinary',
]


MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',  # This should be first
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': ['backend.authentication_classes.JWTAuthentication'],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 5  # Number of posts per page
}
AUTH_USER_MODEL = 'users.User'

CORS_ALLOW_ALL_ORIGINS = True

ROOT_URLCONF = 'backend.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')], 
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'backend.wsgi.application'



DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]



LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True



MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'mediafiles'
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'



cloudinary.config( 
    cloud_name = config('CLOUDINARY_CLOUD_NAME'),
    api_key = config('CLOUDINARY_API_KEY'),
    api_secret = config('CLOUDINARY_API_SECRET'),
)


EMAIL_BACKEND = 'email_backends.mailjet_backend.MailjetBackend'
MAILJET_API_KEY = config('MAILJET_API_KEY')
MAILJET_API_SECRET = config('MAILJET_API_SECRET')
DEFAULT_FROM_EMAIL = 'irshadirshuu1213@gmail.com'

OTP_EXPIRE_TIME = 5


AUTH_TOKEN_EXPIRES_DAY = 30



DEFUALT_PROFILE_IMAGE ="https://static-00.iconduck.com/assets.00/profile-default-icon-2048x2045-u3j7s5nj.png"


AUTH_USER_MODEL = 'users.User'



SITE_URL = 'http://127.0.0.1:8000'


NO_MEDIA_IMAGE = "https://res.cloudinary.com/di4y3lhkc/image/upload/v1728059016/baa0b6kderm7ypuphiyt.avif"
NO_MEDIA_IMAGE = "https://img.freepik.com/premium-vector/no-photo-available-vector-icon-default-image-symbol-picture-coming-soon-web-site-mobile-app_87543-18055.jpg"
```



Please help me for solving this issue . i have been searching for this solution in internet for 5hrs
