# Architecture

## Resources

### Job

The Job resource can be thought of an instance of gcpfind that has or is being run. It maintains the current status: \(Running, Completed, Failed\) of gcpfind, the current stage, and all metadata associated with the job \(such as the images used, and the session.json\).

##### Actions

Cancel

