# Glance

**Service Endpoint URI**: `https://<my-company-platform9-url>/glance`

Platform9 OpenStack Image Service (Glance) provides discovery, registration, and delivery services for instance images. Stored images can be used as a template. It can also be used to store and catalog an unlimited number of backups.
The Glance API is used to retrieve details about images that have been discovered in the image library host, as well as snapshots that have been created using [nova](#create-image).

Check the [Usage](#usage) section for usage details.

## GET /v1/images

```json
{
	"images": [
		{
			"name": "another-test-image",
			"container_format": "bare",
			"disk_format": "qcow2",
			"checksum": "b5b206f3e980806903b1238667af2943",
			"id": "58ce3c31-da87-4051-9ab4-7d2bc25bdaf7",
			"size": 19464192
		},
		{
			"name": "cirros-0.3.2-x86_64-disk.img",
			"container_format": "bare",
			"disk_format": "qcow2",
			"checksum": "64d7c1cd2b6f60c92c14662941cb7913",
			"id": "7084c035-1f93-45d7-8aca-afe74f20cfee",
			"size": 13167616
		},
		...
	]
}
```

Lists public VM images.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
images | array | Contains details of images.
name | string | The name of the image.
container\_format | string | Value of the container format, such as `ovf`, `bare`, `aki`, `ari`, or `ami`.
disk\_format | string | Value of the disk format, such as `qcow2`, `raw`, or `vmdk`.
checksum | string | MD5 checksum of the image file.
id | string | The unique ID of the image.
size | integer | The size of the image in bytes.

**Normal Response Codes**: 200

## GET /v1/images/detail

```json
{
	"images": [
		{
			"status": "active",
			"name": "another-test-image",
			"deleted": false,
			"container_format": "bare",
			"created_at": "2014-09-02T22:25:50",
			"disk_format": "qcow2",
			"updated_at": "2014-09-03T17:36:07",
			"min_disk": 0,
			"protected": false,
			"id": "58ce3c31-da87-4051-9ab4-7d2bc25bdaf7",
			"min_ram": 0,
			"checksum": "b5b206f3e980806903b1238667af2943",
			"owner": null,
			"is_public": true,
			"deleted_at": null,
			"properties": {
				"pf9_addresses": '[{"broadcast": "10.4.255.255", "netmask": "255.255.0.0", "addr": "10.4.118.51"}, {"broadcast": "192.168.122.255", "netmask": "255.255.255.0", "addr": "192.168.122.1"}]',
				"pf9_path": "/v1/images/snapshot/another-test-image",
				"pf9_imagelib_id": "8fb7a370-c69b-4c6d-89ca-a90ca7de8901",
				"pf9_direct_download": "1",
				"pf9_virtual_size": "21474836480",
				"pf9_scheme": "http",
				"pf9_port": "9999"
			},
			"size": 19464192
		},
		...
	]
}
```

Lists all details for available images.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
images | array | Contains details of images.
status | string | The current status of the image, such as `active`, `pending`, or `deleted`.
name | string | The name of the image.
deleted | boolean | Returns `true` if the image was deleted, `false` if not.
container\_format | string | Value of the container format, such as `ovf`, `bare`, `aki`, `ari`, or `ami`.
created_at | dateTime | Time that the image was created.
disk\_format | string | Value of the disk format.
updated_at | dateTime | Time that the image was last updated
protected | boolean | Returns `true` if image is protected, `false` if not.
id | string | The unique ID of the image.
checksum | string | MD5 checksum of the image file.
owner | string | For discovered images, returns `null`. For snapshots, returns the unique ID of the tenant that created the snapshot.
is_public | boolean | Returns `true` if image is public, `false` if private.
deleted_at | dateTime | Time that the image was deleted. Returns `null` if no value exists.
properties | object | Collection of extra properties. `pf9_virtual_size` refers to the size of the virtual disk in bytes. Depending on the disk format, this may be larger than the image size. `pf9_description` refers to the user's description of the image.
size | integer | Size of the image in bytes.

**Normal Response Codes**: 200