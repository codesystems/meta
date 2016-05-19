**Isolation Settings**

Turbo Containers run applications in a light-weight isolated container. Some applications may require to read local files or write to certain local directories.

The default isolation setting for Firefox is **Full** isolation with the exception of local user folders. This is useful for Firefox as downloads automatically go to a users local Downloads folder. 

This setting ensures a user can access their downloaded files outside of the container.
