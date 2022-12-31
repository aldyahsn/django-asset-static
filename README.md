## Config

1. Install whitenoise, django-storage, and boto3

`pip install whitenoise django-storage boto3`


2. Config django-storage, so it can serve uploaded or static assets

```
# DO SPACES
AWS_ACCESS_KEY_ID = 'secret123' # goto API menu in side bar. Generate SPACE Access Key
AWS_SECRET_ACCESS_KEY = 'secretAccess'# goto API menu in side bar. Generate SPACE Secret Access Key
AWS_STORAGE_BUCKET_NAME = 'myspace' #your space object storage name
AWS_S3_ENDPOINT_URL = 'https://sgp1.digitaloceanspaces.com'
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
AWS_LOCATION = 'testing.com' # A directory in your space

MEDIA_URL = 'https://%s/%s/' % (AWS_S3_ENDPOINT_URL, AWS_LOCATION)
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

3. If you want to use whitenoise to serve static assets, add these lines `settings.py`

```python3
MIDDLEWARE = [
    # ...
    "django.middleware.security.SecurityMiddleware",
    "whitenoise.middleware.WhiteNoiseMiddleware",
    # ...
]

# Want forever-cacheable files and compression support? Add this
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"

```


