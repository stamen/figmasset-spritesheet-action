# figmasset-spritesheet-action

Generate a Mapbox GL spritesheet with Figmasset

## Usage

### Inputs

 * `figma-file`: The ID of the Figma file to find icons in
 * `figma-token`: A Figma token that has access to the Figma file
 * `figma-frame`: The ID of a frame in the Figma file to find icons within
 * `dest-dir`: Where to place generated spritesheets

### Outputs

None.

### Example

To generate a spritesheet and sync it with S3:

```yml
name: Generate sprites
on: workflow_dispatch
jobs:
  generate-sprites:
    name: Generate sprites
    runs-on: ubuntu-latest
    steps:
      - id: generate-sprites
        uses: stamen/figmasset-spritesheet-action@main
        with:
          figma-file: ${{ secrets.FIGMA_FILE }}
          figma-token: ${{ secrets.FIGMA_TOKEN }}
          figma-frame: example-frame
          dest-dir: generated-sprites
      - name: Copy sprites to S3
        uses: jakejarvis/s3-sync-action@master
        with:
          args: --follow-symlinks --delete
        env:
          AWS_S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          DEST_DIR: sprites
          SOURCE_DIR: generated-sprites
```
