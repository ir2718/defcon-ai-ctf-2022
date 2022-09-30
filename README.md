# defcon-ai-ctf-2022
My solutions for the [AI Village Capture the Flag @ DEFCON Kaggle competition](https://www.kaggle.com/competitions/ai-village-ctf/).



**HOTDOG**
- **what worked**:  The simplest approach is usually the best to try out first. In this CTF we could upload external data so I used an image of a hotdog to get the flag. This one was pretty easy and didn't require much work.

**MATH CHALLENGE 1**
- **what worked**:  This one was also fairly easy for me. My solution consists of using a k-means clustering algorithm while varying the number of groups and tracking the values of the within-cluster sum of squares (WCSS). Usually as you increase the number of groups this value will go down so I used a heuristic called the [elbow method](https://en.wikipedia.org/wiki/Elbow_method_(clustering)). Once you plot this on a graph you can easily see there's a cuttoff point for `math/clusters1.npy` at 5 and `math/clusters3.npy` at 3. This method didn't give me a decisive answer for `math/clusters2.npy` so I just tried out all the combinations and got to the final solution: 5, 2, 3.

**MATH CHALLENGE 2**
- **what worked**: A bit different from the last one, but still fairly simple. To solve this one I opted for PCA. When using PCA you can easily find out the explained variance for each of the components by the ratio of the related eigenvalue and the sum of all eigenvalues. I wanted to explain as much variance as possible with the least components. For the `math/first_dim1.npy` 3 components were enough, 5 for `math/first_dim2.npy` and 4 for `math/first_dim3.npy`.

**MATH CHALLENGE 3**
- **what worked**:  Also very straightforward. I just used the solution from the previous challenge and got 4 components for `math/second_dim1.npy`, 7 for `math/second_dim2.npy` and 4 for `math/second_dim3.npy`.

**MATH CHALLENGE 4**
- **what worked**: I used part of the solution from the first math challenge and third math challenge. The cutoff point from the WCSS plot is at 5 clusters so I first clustered the data using k-means algorithm. After doing this I filtered each of the clusters I got and used PCA in order to find out what is the dimensionality of every cluster. The dimensionalities ordered by cluster size found using this method are:  5, 4, 3, 1, 2

**FORENSICS**
- **what worked**: Importing the model and calling the summary function. The flag was in the name of the model.

**HONORSTUDENT**
- **what didn't work**: 
     - drawing an `A` using paint
     - programming a perfect `A`
     - filling in the voids starting from the given `F`
- **what worked**: Simply adding noise to the starting `F` image

**BAD TO GOOD**
- **what worked**:  This one was purely trial and error. What worked was a combination of lowering grades, raising the number of absences and demerits and raising the grades. A cool thing I noticed was that there is no validation of the values on the endpoint eg. I expected the the maximum grade to be 100 but you can exceed that like I did.

**BASEBALL**
- **what worked**: Same as with the previous challenge, I used trial and error. I kept adding or subtracting 1 to x or y values of the pitches as long as I kept getting higher confidence that I throw like Henry. Eventually I got the flag by doing this.

**WIFI**
- **hints**: Manifold
- **what didn't work**: 
       - using some manifold techniques such as isomap, LLE, spectral embedding, MDS, t-SNE (these also weren't deterministic)
       - concatenating the vectors of the same tokens into one vector and then trying out the manifold techniques
- **what worked**:  I think this one was one of the coolest problems in the whole competition. After trying out the manifold techniques I turned back to my old ways and used PCA. This time 2 components were enough. I was stuck here for a while, but luckily I thought of visualizing this data using a scatterplot hoping it might help (it did!). I noticed the values looked like a spiral going close to the center of the coordinate system and I wanted to find a way to order the data accordingly. I decided to sort the data using the euclidean distance from the center and then print out the tokens after sorting which turned out to be the solution. I loved this challenge!

**LEAKAGE**
- **what worked**: Since this is a char-RNN model it means that it expects a sequence of characters and it returns a character. My initial guess was to featurize the given username and feed it into the model. I got an integer value and looked up the ASCII encoding for this value. I concatenated the character I got onto the initial string and kept repeating the process. This was the second coolest task for me.

**MURDERBOTS**
- **hints**: Index
- **what didn't work**:
    - hyperparameter optimization, feature selection and scaling the data
- **what worked**: This one gave me headaches before stumbling onto the hint. After seeing the hint I noticed a peculiar thing about the train label indices. The first few indices were `0, 1, 10, 100...` so I figured these must be strings. I sorted them and trained a classifier. Unfortunately my troubles weren't over because I only managed to achieve an accuracy of ~93%. Then I read the objective again and noticed that I have to release "AT LEAST 10 humans". This was important because the string I generated using my classifier wanted to release 12 humans. I don't really care about these humans so I tried out swapping all combinations of 2 humans with murderbots and soon found the solution. Another very neat challenge.

**DEEPFAKE**
- **what didn't work**: 
     - adding random noise to each frame
     - flipping each frame horizontally
- **what worked**: Flipping each frame horizontally and vertically. 😂

**WAF**
- **hints**: spaces will get encoded too, it was a pretty famous exploit
- **what didn't work**:
     - a lot of exploits that were malicious eg. `declare @s varchar(200) `, `a);/usr/bin/id`, `; nc -lvvp 4444 -e /bin/sh;` ...
     - combining spaces into the previously mentioned exploits
- **what worked**: I tried out a lot of exploits from [here](https://github.com/swisskyrepo/PayloadsAllTheThings) and a lot of them gave me the `"MALICIOUS REQUEST CAUGHT BY WAF"` response but I didn't manage to get the flag. Then I stumbled upon the mighty shellshock. It took a few trial and errors adding spaces in between encoded characters and I finally got the flag.

**TOKEN**
- **what didn't work**:
     - brute forcing, plus it was extremely slow
     - checking for examples containing the word SECRET KEY and other words in them
- **what worked**: Looking a bit deeper into the actual data. As I noticed there's a lot of BLANK tokens I thought of looking for examples that contain this token but also other tokens. I found exactly two examples like this and these were the solution.

**SECRET SLOTH**
- **hints**: signal processing/linear algebra
- **what didn't work**:
    - the usual steganography tools: exiftool, binwalk, zsteg, checking the metadata, GIMP filters, XOR with the original image...
- **what worked**: This one took me a while. Once I found the hint I had a general sense of direction and started trying out stuff like singular value decomposition and looking at the histogram of the rgb channels. Still nothing. For some reason the expression signal processsing always means fourier transform to me so I tried that as well. I was also plotting the angle the whole time. After stumbling upon the inverse discrete cosine transform I noticed some gibberish in the lower right part of the red channel but I couldn't recognize the letters. A few days passed but no ideas came to mind. Finally I tried out a few more methods and one of them was the inverse discrete sine transform (who would've thought?) which showed me the flag in the red channel.

**INFERENCE**
- **hints**:  consider the conference this was made for
- **what worked**: My first guess was sending 32x32 images of uppercase letters and then taking the argmax of each of the values and saving that into a dictionary. This turned out to lead me very close to the actual solution. As I was bruteforcing the possible strings using this dictionary I noticed one of them would be `DEFCQN` so I tried out similar flags such as `DEFCON, DEFKON, DEFCOM` etc. A few days later I wanted to double check that it only contains uppercase letters (something along these lines was said in the discord chat) and I finally found the flag: `D3FC0N`.

**THEFT**
- **hints**: people have solved it without using the `encpickle` file (is this a hint?)
- **what didn't work**:
      - trying to read the `encpickle` using every encoding known to man
      - adding random noise
-  **what worked**:  Since salt and theft are somewhat related I wanted to try out using the model prepared for salt to solve theft. Using [this](https://tcode2k16.github.io/blog/posts/picoctf-2018-writeup/general-skills/#solution-20) helped a lot since I don't know much about using keras and tensorflow. The basic idea is to overfit the input image so that it gets predicted as the class that we want. I found the class indices [here](https://deeplearning.cms.waikato.ac.nz/user-guide/class-maps/IMAGENET/).

**HOTTERDOG**
- **what didn't work**:
      - manually combining images of chester and a hot dog in paint
      - blending images of chester and a hot dog
      - adding sauce onto chester 
-  **what worked**:  Once I solved theft I noticed one of the classes used for mobile net was hotdog so I tried the same procedure using the image of chester and the hotdog class and got the flag.

**SALT**
- **what didn't work**:
    - using a too small confidence value
    - adding pepper noise in order to cancel out the salting
- **what worked**: Turns out I was thinking too hard. I just needed to use the same stuff from the theft problem but set the confidence level to 1.0

**CROP 1**
- **what didn't work**:
    - cropping the original image multiple times to get the score as low as possible
    - somehow force idx == 8 and preds.max() around -1 so the values cancel out
    - resizing the image to a smaller size before scoring
- **what worked**: Taking a step back and looking at the scoring function. X_comp is divided by 0b1010 which is 10, then the expected variable is defined as `(25.5 - expected)` and finally sse uses `expected*10` which means the image I need to minimize sse is `255 - X_comp`. After noticing this insight I created a 3x3 grid with each of the subimages being `255-X_comp` but unfortunately this didn't pass the threshold. The culprit was the redness. Finally, if the red channel value was higher than 230 I just replaced it with 220 and became the cropping champion.
