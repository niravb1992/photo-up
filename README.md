About
======
photo-up is a photo uploading app for [Django](https://www.djangoproject.com/) projects. It utilizes HTML5's [FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader), [FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList), and [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData) APIs to accomplish the choosing, uploading, and upload progress tracking of photos on the frontend. 

The purpose of this project is to explore and provide a purely JavaScript based mechanism for uploading files.

Features
========
* Select multiple photos to upload
* See a preview of the selected photos
* Remove selected photos
* Upload photos via AJAX
* View upload progress

Prerequisites
==============
1. Python + Django installation
2. [Pillow](https://github.com/python-pillow/Pillow) module installed
3. An existing Django project
4. A browser that has support for FileReader, FileList, FormData objects and XHR2

Installation
======================
1. Download the latest release and un-archive it
2. In the above, copy the photoupweb folder to the root of your django project (where you have your manage.py)
3. Make the following changes in your settings.py:

   ```
    TEMPLATE_DIRS = (
        # everything else...
        os.path.join(BASE_DIR, 'photoupweb', 'templates')
    )

    STATICFILES_DIRS = (
        # everything else...
        os.path.join(BASE_DIR, 'photoupweb', 'static'),
    )

    INSTALLED_APPS = (
        # everything else...
        'photoupweb',
    )
    
    # If you don't have MEDIA_URL and MEDIA_ROOT settings already
    
    MEDIA_URL = '/media/'
    
    MEDIA_ROOT = os.path.join(BASE_DIR,...) 
   ```

4. Make the following changes in urls.py:

   ```
    # Add this at the top with the other import statements (if you don't have it already)
    from django.conf.urls import patterns
   
    urlpatterns += patterns('photoupweb.views',
    url(r'^photoup/$', 'index'),
    url(r'^photoup/upload/$', 'upload'),
    url(r'^photoup/view/(?P<photo_id>\d+)/$','view_photo')
    )
   ```

5. If you also want to be able to view uploaded photos in DEBUG mode, you can also add this to urls.py

   ```
    # Add these at the top with other import statements (if you don't have them already)
    from django.conf import settings
    from django.conf.urls.static import static

    # Add this at the end
    if settings.DEBUG:
        urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
   ```

6. Run migrate:

   ```
    python manage.py migrate
   ```

7. That's it! When you run your django project, you can access the photo uploader at /photoup/

Notes
==============
* Uploaded photos are stored in a 'photoup' folder in the MEDIA_ROOT location.
* For each uploaded photo, an instance of the PhotoUpload model is created.
* To view an uploaded photo, use the url /photoup/view/some photo id
