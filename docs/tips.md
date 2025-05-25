# Tips & Tricks

## TYPE_CHECKING

Do you use `TYPE_CHECKING` in bricks? `TYPE_CHECKING` is a special constant that is `True` for static type checkers,
but `False` at runtime.

Example:
``` python
if TYPE_CHECKING:
    from mypy_boto3_s3.service_resource import Object as S3Object
```

The `poly check` command will identify the import and expect it to be defined as a dependency.
Usually, these dev-specific dependencies are added as dev-dependencies (and that is correct).

This means that the `poly check` command will report it as a missing dependency.


### Solution
To resolve this, you can add _optional dependencies_ in the projects that use the brick with the TYPE_CHECKING constant.

Example, using the dev-only "boto3-stubs" library:
``` python
# add the dev-dependencies for every project that is using the brick
[project.optional-dependencies]
local-dev = ["boto3-stubs[s3]"]
```
