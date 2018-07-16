<a name="image" />

## Image

Image fields:

field name | type | required | description
--- | --- | --- | ---
id | `ID` | True | image `relay_id`
checksum | String | True | image hash
originalHeight | Int | True | original image height
originalWidth | Int  | True | original image width
originalUrl | String | True | original image CDN url
mainColor | String | True | Main color in this image in hex format
type | String | True | image format, `png/jpeg`
versions | List | True | All available versions of this image

### Upload Image

You can get the image_id by using the UploadImage method below.

```
mutation uploadImage($image: File!) {
  uploadImage(file: $image) {
    image {
      id
      originalWidth
      originalHeight
      originalUrl
      mainColor
      checksum
      versions {
        filename
        height
        width
      }
    }
  }
}    
```

The expected result is as follows.

```
{
  "data": {
    "uploadImage": {
      "image": {
        "checksum": "a6005826c6e1b99cf46001d50ff3622f", 
        "id": "mDDlGkl4YXDuQ", 
        "mainColor": "#CDCDCD", 
        "originalHeight": 1644, 
        "originalUrl": "https://cdn.shoppo.com/images/a6005826c6e1b99cf46001d50ff3622f.JPEG", 
        "originalWidth": 2000, 
        "versions": [
          {
            "filename": "a6005826c6e1b99cf46001d50ff3622f.JPEG", 
            "height": 1644, 
            "width": 2000
          }, 
          {
            "filename": "a6005826c6e1b99cf46001d50ff3622f-100x100.JPEG", 
            "height": 100, 
            "width": 121
          }, 
          {
            "filename": "a6005826c6e1b99cf46001d50ff3622f-400x400.JPEG", 
            "height": 400, 
            "width": 486
          }, 
          {
            "filename": "a6005826c6e1b99cf46001d50ff3622f-1000x1000.JPEG", 
            "height": 1000, 
            "width": 1216
          }
        ]
      }
    }
  }
}
```


<a name="video" />

## Video

Video fields:

field name | type | required | description
--- | --- | --- | ---
id | `ID` | True | video `relay_id`
checksum | String | True | video hash
rowUrl | String | True | original video Url
