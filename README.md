# Stacked Hourglass model : TensorFlow implementation
Tensorflow implementation of Stacked Hourglass Networks for Human Pose Estimation by A.Newell et al.
## Based on
[Stacked Hourglass Networks for Human Pose Estimation](https://arxiv.org/abs/1603.06937) -- A.Newell et al.
## Status
This is a WIP repo

## Config File
A 'config.cgf' is present in the directory.It contains all the variables needed to tweak the model.
	
	training_txt_file : Path to TEXT file containing information about images
	img_directory : Path to folder containing images
	img_size : Size of input Image /!\ DO NOT CHANGE THIS PARAMETER (256 default value)
	hm_size : Size of output heatMap /!\ DO NOT CHANGE THIS PARAMETER (64 default value)
	num_joints : Number of joints considered
	joint_list: List of joint name
	name : Name of trained model
	nFeats: Number of Features/Channels in the convolution layers (256 / 512 are good but you can set whatever you need )
	nStacks: Number of Stacks (4 to make the faster prediction, 8 stacks are used in the paper)
	nModules : NOT USED
	nLow : Number of downsampling in one stack (default: 4 => dim 64->4)
	dropout_rate : Percentage of neurons deactivated at the end of Hourglass Module (Used for training)
	batch_size : Size of training batch (8/16/32 are good values depending on your hardware)
	nEpochs : Number of training epochs
	epoch_size : Iteration in a single epoch
	learning_rate: Starting Learning Rate
	learning_rate_decay: Decay applied to learning rate (in ]0,1], 0 not included), set to 1 if you don't want decay learning rate. (Usually, keep decay between 0.9 and 0.99)
	decay_step : Step to apply decay to learning rate
	valid_iteration : Number of prediction made on validation set after one epoch (valid_iteration >= 1)
	log_dir_test : Directory to Test Log file
	log_dir_train : Directory to Train Log file
	saver_step : Step to write in train log files (saver_step < epoch_size)
	saver_directory: Directory to save trained Model
## DataSet
To create a dataset you need to put every images of your set on the 'img_directory'.
Add information about your images into the 'training_txt_file':

EXAMPLE:

		055906773.jpgA 391 195 596 567 556 567 507 480 524 382 549 385 534 492 596 566 537 384 501 248 477 236 391 195 479 333 515 315 492 252 510 243 564 317 484 350
		055906773.jpgB 339 284 401 379 386 375 362 358 391 346 401 345 370 359 400 379 396 346 378 309 370 304 339 284 342 346 369 336 369 310 387 308 395 344 369 355
		026504353.jpgA 566 159 680 490 623 442 584 379 616 322 658 323 658 398 666 490 637 323 645 230 645 212 647 159 590 273 566 259 611 222 679 237 680 287 652 301
		026504353.jpgB 555 137 623 375 586 375 577 310 580 266 609 263 609 310 604 374 595 265 581 184 577 176 555 137 570 261 559 238 561 193 601 174 623 207 591 233
		026504353.jpgC 502 268 584 434 555 434 546 390 536 354 560 346 570 387 584 430 548 350 531 303 527 297 507 268 519 346 502 340 512 307 550 299 561 331 535 348
In this example we consider 16 joints

	['r_anckle', 'r_knee', 'r_hip', 'l_hip', 'l_knee', 'l_anckle', 'pelvis', 'thorax', 'neck', 'head', 'r_wrist', 'r_elbow', 'r_shoulder', 'l_shoulder', 'l_elbow', 'l_wrist']
The text file is formalized as follow:

	image_name[LETTER] x_box_min y_box_min x_box_max y_box_max x1 y1 x2 y2 x3 y3 ...
	image_name is the file name
	[LETTER] Indicates the person considered
	(x_box_min y_box_min x_box_max y_box_max) Is the bounding box of the considered person in the scene
	(x1 y1 x2 y2 x3 y3 ...) is the list of coordinates of every joints

This data formalism consider a maximum of 10 persons in a single image (You can tweak the datagen.py file to consider more persons)

/!\Missing part or values must be marked as -1
