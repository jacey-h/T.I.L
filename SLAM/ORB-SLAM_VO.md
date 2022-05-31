
## ORB-SLAM

# 1. ORB

- with loopclosing
    
    ![Selection_021](https://user-images.githubusercontent.com/81483791/171212608-75da3b87-d771-49e0-bec0-e1375c234419.png)

- without loopclosing
    
    ![Selection_022](https://user-images.githubusercontent.com/81483791/171212623-1a2af64e-92c1-42d7-b31e-952a86d98552.png)

- visual odometry compare until same sequences
    
    ![Selection_020](https://user-images.githubusercontent.com/81483791/171212683-a8ac4854-f45f-4adb-a75b-9cb87945bc64.png)

 almost same when the threshold is 5 or 10

# 2. Visual odometry

- base

![Selection_013](https://user-images.githubusercontent.com/81483791/171212724-e2d18be4-1649-49c8-881d-91c35097006b.png)

edit this file 

they have image, keypoint, threshold

![Selection_015](https://user-images.githubusercontent.com/81483791/171212744-11329333-9b64-4aef-9215-6f3230a2c851.png)

- threshold =100
    
    ![Selection_016](https://user-images.githubusercontent.com/81483791/171212769-96cd62e6-8ca4-44d4-a3c0-64f430b5c2c2.png)

- threshold = 10
    
    ![Selection_017](https://user-images.githubusercontent.com/81483791/171213266-977ad215-56ae-45eb-b31e-6e06002cdf51.png)



- threshold = 5
    
    ![Selection_018](https://user-images.githubusercontent.com/81483791/171212823-cb0bcd1a-5b8e-441d-b28b-fd4c52d7635a.png)


- threshold = 150
    
    ![Selection_019](https://user-images.githubusercontent.com/81483791/171212841-d78f27a9-4143-4e55-ba87-048b3ae24aa2.png)

    maximum fram change 4540 instead of 1000
    
    1. threshold 5 / true
        
        ![Selection_024](https://user-images.githubusercontent.com/81483791/171212869-9ecc4cbb-61c0-4a1a-96e3-650e10bedd59.png)

1. threshold 5 / false
    
    ![Selection_025](https://user-images.githubusercontent.com/81483791/171212891-530b3d55-f6e2-4a02-aa41-42515a14b9d3.png)


00 data calibration
    ![Selection_023](https://user-images.githubusercontent.com/81483791/171212967-0736d949-117d-4f57-90af-d59ead44554f.png)

