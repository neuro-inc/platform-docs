# Images

The Images app provides a central location for viewing properties of image assets used in deployments.

The Images application is accessible from the left-side navigation menu in the Apolo Console. All container images associated with the selected project and cluster, along with detailed information about each image. Each image is represented by a path.

A search field at the top of the list allows you to quickly locate specific images by name or path.

On the right side of the screen, selecting an image displays detailed information about it, including:

- *Image Tags*. Tags associated with the image. Useful for version control (e.g., latest).
- *Image Path*. The full path to the image within the Apolo storage.
- *Creation Date*. The date when the image was added. 
- *Size*. Total size of the image.
- *Copy Icons*. Allows the user to copy either the image path or tag to the clipboard for easy reference.

For working with images into storage (pushing, pulling, removing, etc.), you should use [image CLI commands](/references/cli-reference/image.md).
