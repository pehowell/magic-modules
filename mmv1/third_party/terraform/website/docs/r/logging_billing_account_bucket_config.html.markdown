---
subcategory: "Cloud (Stackdriver) Logging"
page_title: "Google: google_logging_billing_account_bucket_config"
description: |-
  Manages a billing account level logging bucket config.
---

# google\_logging\_billing_account\_bucket\_config

Manages a billing account level logging bucket config. For more information see
[the official logging documentation](https://cloud.google.com/logging/docs/) and
[Storing Logs](https://cloud.google.com/logging/docs/storage).

~> **Note:** Logging buckets are automatically created for a given folder, project, organization, billingAccount and cannot be deleted. Creating a resource of this type will acquire and update the resource that already exists at the desired location. These buckets cannot be removed so deleting this resource will remove the bucket config from your terraform state but will leave the logging bucket unchanged. The buckets that are currently automatically created are "_Default" and "_Required".

## Example Usage

```hcl
data "google_billing_account" "default" {
	billing_account = "00AA00-000AAA-00AA0A"
}

resource "google_logging_billing_account_bucket_config" "basic" {
	billing_account    = data.google_billing_account.default.billing_account
	location  = "global"
	retention_days = 30
	bucket_id = "_Default"
}
```

## Argument Reference

The following arguments are supported:

* `billing_account` - (Required) The parent resource that contains the logging bucket.

* `location` - (Required) The location of the bucket.

* `bucket_id` - (Required) The name of the logging bucket. Logging automatically creates two log buckets: `_Required` and `_Default`.

* `description` - (Optional) Describes this bucket.

* `retention_days` - (Optional) Logs will be retained by default for this amount of time, after which they will automatically be deleted. The minimum retention period is 1 day. If this value is set to zero at bucket creation time, the default time of 30 days will be used. Bucket retention can not be increased on buckets outside of projects.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are
exported:

* `id` - an identifier for the resource with format `projects/{{project}}/locations/{{location}}/buckets/{{bucket}}`

* `name` -  The resource name of the bucket. For example: "projects/my-project-id/locations/my-location/buckets/my-bucket-id"

* `lifecycle_state` -  The bucket's lifecycle such as active or deleted. See [LifecycleState](https://cloud.google.com/logging/docs/reference/v2/rest/v2/billingAccounts.buckets#LogBucket.LifecycleState).

## Import

This resource can be imported using the following format:

```
$ terraform import google_logging_billing_account_bucket_config.default billingAccounts/{{billingAccount}}/locations/{{location}}/buckets/{{bucket_id}}
```
