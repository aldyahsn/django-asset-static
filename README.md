## Config

1. Install whitenoise, django-storage, and boto3

`pip install whitenoise django-storage boto3`


2. Config django-storage, so it can serve uploaded or static assets

```python3

# ----------------------------
# DIGITAL OCEAN SPACES CONFIG
# ----------------------------
AWS_ACCESS_KEY_ID = 'secret123' # goto API menu in side bar. Generate SPACE Access Key
AWS_SECRET_ACCESS_KEY = 'secretAccess'# goto API menu in side bar. Generate SPACE Secret Access Key
AWS_STORAGE_BUCKET_NAME = 'myspace' #your space object storage name
AWS_S3_ENDPOINT_URL = 'https://sgp1.digitaloceanspaces.com'
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
AWS_LOCATION = 'testing.com' # A directory in your space


# ----------------------------
# ASSETS CONFIG
# ----------------------------
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'

STATIC_URL = 'static/' # static url
STATIC_ROOT = BASE_DIR.parent / "public/static" # sample location assets
STATICFILES_DIRS = [
    BASE_DIR.parent / "assets", # another sample location assets
]


# MEDIA_URL = 'media/'
MEDIA_URL = 'https://%s/%s/' % (AWS_S3_ENDPOINT_URL, AWS_LOCATION) # Media url
# MEDIA_ROOT = BASE_DIR.parent / "public/media"

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


