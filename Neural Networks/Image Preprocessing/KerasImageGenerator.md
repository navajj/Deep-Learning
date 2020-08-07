***steps_per_epoch***: Integer. Total number of steps (batches of samples) to yield from generator before declaring one epoch finished and starting the next epoch. It should typically be equal to ceil(num_samples / batch_size). Optional for Sequence: if unspecified, will use the len(generator) as a number of steps.

You can set it equal to **num_samples/batch_size**, which is the standard choice.

However, steps_per_epoch give you the chance to "trick" the generator when updating the learning rate using ReduceLROnPlateuau() callback, because this callback checks the drop of the loss once each epoch has finished. If the loss has stagnated for a patience number of consecutive epochs, the callback decreases the learning rate to "slow-cook" the network. If your dataset is huge, as it is usually the case when you need to use generators, you would probably like to decay the learning rate within a single epoch (since it includes a big number of data). This can be achieved by setting steps_per_epoch to a value that is less than num_samples // batch_size without affecting the overall number of training epochs of your model.

Imagine this case as using mini-epochs within your normal epochs to change the learning rate because your loss has stagnated. Very useful in my applications.
