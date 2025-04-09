% addpath('path_to_where_you_saved\');
addpath('C:\Users\ariff\OneDrive\Desktop\ComBatDepend');

% ================== ComBat Tutorial ==================

%{
The combat() function is the main function. 

It requires two mandatory arguments: 

1. A data matrix (p x n) 
   p rows = # of features
            features are the variable that you are measuring
            Ex: number of genes, number of white matter tracts, number of
            voxels per scan.
   n columns = # of participants

2. A numeric or character vector batch of length n 
   *Indicates the site/scanner/study id.

Let's say we extracted mean FA values from 20 white matter tracts for 
100 participants (p = 20, n = 100). Then:

data = [ 20 x 100 ] matrix of FA values
batch = [ 1 1 1 ... 2 2 2 ] (vector of length 100 showing scanner/site)

And then we can run:
combat_data = combat(data, batch);

Combat also has optional mod argument. The optional mod argument is a 
matrix of biological covariates (i.e. information about your participants 
that you do not want ComBat to remove from the data). These are variables you 
think are biologically meaningful and might explain real differences 
in the data). 

To preserve the effects of variables you care about (like age, sex, diagnosis), 
you include them in mod.

Example including mod:
mod = [age', sex', diagnosis'];  % n x k matrix
mod = age;  % A column vector (n x 1)
combat_data = combat(data, batch, mod);

%}

% ================== Example 1 ==================

% Let's simulate an imaging dataset with n=10 participants, 
% acquired on 2 scanners, with 5 participants each, 
% with p = 10000 voxels per scan

p=10000;
n=10;
batch = [1 1 1 1 1 2 2 2 2 2]; %Batch variable for the scanner id
data = randn(p,n); %Random data matrix

% Let's also simulate two biological covariates, age and sex:
age = [82 70 68 66 80 69 72 76 74 80]'; % Continuous variable
sex = [1 2 1 2 1 2 1 2 1 2]'; % Categorical variable (1 for females, 2 for males)

% We need to convert categorical variables like sex into dummy variables 
% (also called one-hot encoding).
% This is done because ComBat already includes an intercept, so you must 
% leave out one category to avoid multicollinearity (a problem in 
% regression where variables are linearly dependent).
sex = dummyvar(sex);
%If sex originally had two categories (e.g., 1 = female, 2 = male), then we
%convert it to:
% [1 0] for female
% [0 1] for male

% About sex(:,2):
% This selects only one of the two dummy columns — here, the one for male.

mod = [age sex(:,2)];
% Combines age and the dummy variable for males into a model matrix.
% Now mod looks like this:
% [ age_1   is_male_1
%  age_2   is_male_2
%  ...     ...
% ]

% And now we can use the function combat to harmonize the data across 
% the 2 scanners using parametric adjustements:

data_harmonized = combat(data, batch, mod, 1);

% And the fourth argument, 1, indicates parametric adjustements. 
% To perform non-parametric adjustments, replace 1 by 0 as follows:

% data_harmonized = combat(data, batch, mod, 0);

% To use ComBat without adjusting for biological variables, simply set
% mod=[];

% ================== Example 2 ==================
% What if we had 3 biological covariates: age, sex (male or female) and 
% disease (healthy, mci, or AD).

age = [82 70 68 66 80 69 72 76 74 80]'; % Continuous variable

sex = [1 2 1 2 1 2 1 2 1 2]'; % Categorical variable (1 for females, 2 for males)
sex = dummyvar(sex)

disease = {'ad'; 'healthy';'healthy';'healthy';'mci';'mci';'healthy'; 'ad'; 'ad';'mci'};
disease = categorical(disease);
disease = dummyvar(disease);
% This generates one-hot encoding for each category (just like with sex earlier)
% , so now you get something like:
% 'ad'	- [1 0 0]
% 'healthy'	- [0 1 0]
% 'mci'	- [0 0 1]

%So after dummyvar, disease becomes a 10 x 3 matrix like:
% [1 0 0; 0 1 0; 0 1 0; ... ]

% To build the model matrix, we need to omit one category (reference group) 
% for each of the categorical variables (sex and disease):

mod = [age sex(:,2) disease(:,2:3)];
% Here, we only omitted the first catergoy for sex and disease. 
% disease(:, 2:3) -> : in MATLAB means "all rows" and 2:3 means 
% columns 2 through 3

% =================== Visualizing Differences =========================

% =========== Visualization: Before vs. After Harmonization ===============
% Let's randomly pick one voxel (feature) to illustrate the impact of 
% harmonization.
% We'll plot boxplots of that voxel’s values across scanners before 
% and after ComBat.
% You should see that after harmonization, differences between scanners are 
% reduced (i.e., boxplots align better), while biological variability 
% is preserved.

% Choose a random voxel/feature to plot
voxel_idx = randi(p);  % random voxel index

% Extract data for that voxel (before and after)
raw_values = data(voxel_idx, :);
harm_values = data_harmonized(voxel_idx, :);

% Group by batch (site/scanner)
figure;
subplot(1,2,1);
boxplot(raw_values, batch);
title(['Raw Data - Voxel ', num2str(voxel_idx)]);
xlabel('Scanner ID');
ylabel('FA Value');
ylim([-3 3]);

subplot(1,2,2);
boxplot(harm_values, batch);
title(['Harmonized Data - Voxel ', num2str(voxel_idx)]);
xlabel('Scanner ID');
ylabel('FA Value');
ylim([-3 3]);

sgtitle('Comparison of Raw vs. ComBat-Harmonized FA Values by Scanner');
