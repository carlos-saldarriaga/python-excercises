# S3 Objects Fetcher

This document describes a Python function that fetches objects from an AWS S3 bucket based on a given prefix using the `boto3` library.

## Function Overview

The function `get_s3_objects` retrieves objects from an S3 bucket that match a specified prefix. Instead of returning a complete list of matching objects, it uses the `yield` statement to produce a generator, allowing for lazy evaluation and improved memory efficiency.

### Parameters

- `bucket`: Name of the S3 bucket.
- `prefix`: (Optional) The prefix string to filter the S3 objects. Defaults to an empty string.

## Code Snippet

```python
def get_s3_objects(bucket, prefix=''):
    s3 = boto3.client('s3')

    kwargs = {'Bucket': bucket}
    next_token = None
    if prefix:
        kwargs['Prefix'] = prefix

    while True:
        if next_token:
            kwargs['ContinuationToken'] = next_token
        resp = s3.list_objects_v2(**kwargs)
        contents = resp.get('Contents', [])
        for obj in contents:
            key = obj['Key']
            if key.startswith(prefix):
                yield obj
        next_token = resp.get('NextContinuationToken', None)

        if not next_token:
            break
```

## Function Breakdown

1. Establish an S3 client using `boto3`.
2. Configure the S3 `list_objects_v2` request with the bucket name and optional prefix.
3. Loop through the paginated results of the `list_objects_v2` call. AWS S3 may paginate results if there are many objects.
4. For each object in the returned contents, check if its key starts with the given prefix.
5. If it matches, the object is yielded, allowing the caller to process one object at a time.
6. Check for a continuation token in the response to see if there are more paginated results to process. If not, exit the loop.

## Usage

This function returns a generator. To get all matching S3 objects as a list:

```python
objects = list(get_s3_objects('my_bucket_name', 'my_prefix/'))
```
