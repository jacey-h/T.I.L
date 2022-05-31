# 슬램

복습: No
작성일시: 2022년 6월 1일 오전 12:24

## ORB-SLAM

# 1. ORB

- with loopclosing
    
    ![Selection_021.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_021.png)
    
- without loopclosing
    
    ![Selection_022.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_022.png)
    

- visual odometry compare until same sequences
    
    ![Selection_020.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_020.png)
    

 almost same when the threshold is 5 or 10

# 2. Visual odometry

- base

![Selection_013.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_013.png)

edit this file 

they have image, keypoint, threshold

![Selection_015.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_015.png)

- threshold =100
    
    ![Selection_016.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_016.png)
    

- threshold = 10
    
    ![Selection_017.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_017.png)
    
- threshold = 5
    
    ![Selection_018.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_018.png)
    

- threshold = 150
    
    ![Selection_019.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_019.png)
    
    maximum fram change 4540 instead of 1000
    
    1. threshold 5 / true
        
        ![Selection_024.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_024.png)
        
1. threshold 5 / false
    
    ![Selection_025.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_025.png)
    

00 data calibration

![Selection_023.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_023.png)

![Selection_023.png](%E1%84%89%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%86%B7%201b1e1014dd9a4aa295a3a3be44a6afee/Selection_023.png)