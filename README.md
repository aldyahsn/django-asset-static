## Config

1. Install libraries
`pipenv install django-storages boto3`

2. Add file storage config
```python3
from django.conf import settings
from storages.backends.s3boto3 import S3Boto3Storage


class StaticStorage(S3Boto3Storage):
    location = 'testing.com/public/static'  # file location in space
    default_acl = 'public-read'


class PublicMediaStorage(S3Boto3Storage):
    location = 'testing.com/public/media'
    default_acl = 'public-read'
    file_overwrite = False


class PrivateMediaStorage(S3Boto3Storage):
    location = 'private'
    default_acl = 'private'
    file_overwrite = False
    custom_domain = False 
```

3. Set config in `settings.py`
```python3
# ----------------------------
# DIGITAL OCEAN SPACES CONFIG
# ----------------------------
AWS_ACCESS_KEY_ID = 'your-key' # goto API menu in side bar. Generate SPACE Access Key
AWS_SECRET_ACCESS_KEY = 'your-secret-key'# goto API menu in side bar. Generate SPACE Secret Access Key
AWS_STORAGE_BUCKET_NAME = 'your-space-name' #your space object storage name
AWS_S3_ENDPOINT_URL = 'https://sgp1.digitaloceanspaces.com'
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
# ----------------------------
# ASSETS CONFIG
# ----------------------------

STATICFILES_STORAGE = 'core.storage_backends.StaticStorage'
DEFAULT_FILE_STORAGE = 'core.storage_backends.PublicMediaStorage'

# Below settings will be ignored when using above storages 
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR.parent / 'public/static'
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR.parent / 'public/media'

```
