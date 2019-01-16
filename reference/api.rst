API: Messages
^^^^^^^^^^^^^



* TaskAddedEvent: This is sent to indicate that a new task has been added to the
  queue. The shovel node will try and split the media file into chunks according
  the supplied ffmpeg arguments. When that is done, it'll send a
  TaskCompletedEvent.
* TaskCancelledEvent: This is sent when the user cancels the current task. All
  encoding slices will be interrupted and cleared from the queue.
* TaskCompletedEvent: This is the confirmation that the media file is split in
  slices and that it's ready for transcoding.
* SliceAddedEvent: This event is placed in queue for each slice. It holds all
  the information necessary to transcode the slice. When the slice is complete,
  the message will be ack'ed, indicating that the slice is transcoded.
* SliceCompletedEvent: This message is sent additionally after the Ack of
  SliceAddedEvent and is used to inform the master about the progress of the
  task.
