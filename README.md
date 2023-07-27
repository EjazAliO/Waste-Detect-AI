# Waste-Detect-AI
Waste Deciding Model

This model is used to predict the type of waste an item is based on its characteristics. It is trained on an imagenet Resnet-18 model using transfer learning. The idea is that many people don't know if an Item is recycled or trash. This AI would decide that for you.

![image](https://github.com/EjazAliO/Waste-Detect-AI/assets/140644020/292708ec-d523-456a-97e1-008d985837b1)

![image](https://github.com/EjazAliO/Waste-Detect-AI/assets/140644020/f09c5fc0-a127-452f-a68b-f206dee1394a)

![image](https://github.com/EjazAliO/Waste-Detect-AI/assets/140644020/e966be34-30a5-4c6f-bb52-96315da3cea1)




## The Algorithm
The algorithm is used by importing an image input of a waste Item into the Jetson Nano. The program uses a 2GB Jetson Nano. The algorithm will then apply its thinking to decide if the waste is organic, recyclable, or trash. Then the user can use this information to through it away in the correct bin.

## Running this project
Launch VS Code.
1. Click on the small green icon at the bottom left of your screen to access the SSH menu.
2. Select + Add New SSH Host to add a new host.
3. Enter ssh nvidia@x.x.x.x, replacing x.x.x.x.x with the IP address you usually use in Putty or terminal to connect to the Nano.
4. Pick the first configuration file.
5. Click Connect in the prompted window.
6. Choose Linux as the operating system when asked.
7. If you're asked to continue, click Continue.
8. You'll be asked for a password after connecting to the Nano. Input your Nano password and hit Enter.
9. Select Open Folder and navigate to jetson-inference. Input your password again if required.
10. Click Yes, I trust the authors to access and start working on your projects in this directory.


## Preparing the Dataset
1. Navigate to jetson-inference/python/training/classification/data.
2. Extract the dataset ZIP file.
3. Inside jetson-inference/python/training/classification/data, create a new folder called waste_detect. Inside waste_detect, add three folders: test, train, val, and a file named labels.txt.
4. In the train directory inside waste_detect, create 3 folders named recyclable, organic, and trash.
5. Copy these folders to the val and test directories.
6. Distribute the images from your ZIP file among these folders, with 80% in the train folder, 10% in the val folder, and 10% in the test folder for each waste type. Unfortunately, this will be a manual task and may take some time.

## Running the Docker Container
1. Go to the jetson-inference folder and run ./docker/run.sh.
2. Once inside the Docker container, navigate to jetson-inference/python/training/classification.

## Training the Neural Network
1. Run the training script with the following command: python3 train.py --model-dir=models/ANY_NAME_YOU_WANT --batch-size=4 --workers=4 --epoch=1 data/waste_detect Replace ANY_NAME_YOU_WANT with your desired output file name. This process may take quite some time.
2. You can stop the process at any time using Ctl+C and resume it later using the --resume and --epoch-start flags.

## Testing the Trained Network on Images
1. Exit the Docker container by pressing Ctrl + D in the terminal.
2. On your Nano, navigate to jetson-inference/python/training/classification.
3. Check if the model exists on the Nano by executing ls models/ANY_NAME_YOU_WANT/. You should see a file named resnet18.onnx.
4. Set the NET and DATASET variables: NET=models/ANY_NAME_YOU_WANT DATASET=data/waste_detect
5. Run this command to see how the model works on an image from the test folder: imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/recyclable/PICK_AN_IMAGE.jpg PICK_A_NAME_FOR_THE_IMAGE.jpg. Keep in mind that you are able to change recyclable to any waste you want, you are able to pick any test image by changing PICK_AN_IMAGE.jpg and are able to change the name of the output image name by changing PICK_A_NAME_FOR_THE_IMAGE.jpg. 6. Launch Visual Studio Code to view the image output (located in the classification folder). Remember to replace ANY_NAME_YOU_WANT with the name you gave your model while training.

https://youtu.be/6ufqk7kHM4k Video Link
