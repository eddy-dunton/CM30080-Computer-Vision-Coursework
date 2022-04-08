%% Notes
% may have to run a couple of times to see correct results, unsure as to
% why the first run often fails, may be issue with library.
% This is currently experimenting on a single emoji and differing scenes

%% Read in images
% read in images and convert to grayscale, training images are read with
% transparency

% import image as transparent
[boxImg, ~, alpha] = imread('1.png');
boxImg = rgb2gray(boxImg);
figure;
imshow(boxImg);
title('Reference Training Image');

%% Test cases and results

% % identical image (transparent background)
% working as expected
[sceneImg, ~, alpha] = imread('1.png');

% % identical image with white background
% working as expected
% [sceneImg, ~, alpha] = imread('2.png');

% % 50% downscale
% working as expected
% [sceneImg, ~, alpha] = imread('3.png');

% % 90% downscale
% issue with sizing? no feature matches
% [sceneImg, ~, alpha] = imread('4.png');

% % 50% with rotation
% working up to removing inliers, where there is a failure to find enough
% inliers, even though the are plenty present
% [sceneImg, ~, alpha] = imread('5.png');

% % 50% downscale with another object
% working as expected
% [sceneImg, ~, alpha] = imread('6.png');

% % Task test case with no rotation
% found features however no matches
% [sceneImg, ~, alpha] = imread('7.png');

% % Task test case with rotation
% found features however no matches
% [sceneImg, ~, alpha] = imread('8.png');


sceneImg = rgb2gray(sceneImg);
figure;
imshow(sceneImg);
title('Testing Scene Image');

%% Extracting Keypoints
boxPoints = detectSURFFeatures(boxImg);
scenePoints = detectSURFFeatures(sceneImg);

figure;
imshow(boxImg);
title('100 Strongest Feature Points from Reference Image');
hold on;
plot(selectStrongest(boxPoints, 100));

figure;
imshow(sceneImg);
title('300 Strongest Feature Points from Scene Image');
hold on;
plot(selectStrongest(scenePoints, 300));

%% Extracting Features
[boxFeatures, boxPoints] = extractFeatures(boxImg, boxPoints);
[sceneFeatures, scenePoints] = extractFeatures(sceneImg, scenePoints);

boxPairs = matchFeatures(boxFeatures, sceneFeatures);

%% Match Features
matchedBoxPoints = boxPoints(boxPairs(:, 1), :);
matchedScenePoints = scenePoints(boxPairs(:, 2), :);
figure;
showMatchedFeatures(boxImg, sceneImg, matchedBoxPoints, matchedScenePoints, 'montage');
title('Putatively Matched Points (Including Outliers)');

[tform, inlierIdx] = estimateGeometricTransform2D(matchedBoxPoints, matchedScenePoints, 'affine');
inlierImgPoints = matchedBoxPoints(inlierIdx, :);
inlierScenePoints = matchedScenePoints(inlierIdx, :);
[tform, inlierIndx] = estimateGeometricTransform2D(matchedBoxPoints, matchedScenePoints, ...
     'affine');

figure;
showMatchedFeatures(boxImg, sceneImg, inlierImgPoints, inlierScenePoints, 'montage');
title('Matched Points (Inliers Only)');

boxPolygon = [1, 1;
    size(boxImg, 2), 1;
    size(boxImg, 2), size(boxImg, 1);
    1, 1];

newBoxPolygon = transformPointsForward(tform, boxPolygon);




