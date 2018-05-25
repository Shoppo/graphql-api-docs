## Image

Image fields:

field name | type | required | description
--- | --- | --- |
id | `ID` | True | image `relay_id`
checksum | String | True | image hash
originalHeight | Int | True | original image height
originalWidth | Int  | True | original image width
originalUrl | String | True | original image CDN url
type | String | True | image format, `png/jpeg`

## Video

Video fields:

field name | type | required | description
--- | --- | --- |
id | `ID` | True | video `relay_id`
checksum | String | True | video hash
rowUrl | String | True | original video Url
