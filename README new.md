Image Denoising

    Gaussian Image Denoising
        Training
        Evaluation
            Grayscale blind image denoising testing
            Grayscale non-blind image denoising testing
            Color blind image denoising testing
            Color non-blind image denoising testing
    Real Image Denoising
        Training
        Evaluation
            Testing on SIDD dataset
            Testing on DND dataset

Gaussian Image Denoising

    Blind Denoising: One model to handle various noise levels
    Non-Blind Denoising: Separate models for each noise level

Training

    Download training (DIV2K, Flickr2K, WED, BSD) and testing datasets, run

python download_data.py --data train-test --noise gaussian

    Generate image patches from full-resolution training images, run

python generate_patches_dfwb.py 

    Train Restormer for grayscale blind image denoising, run

cd Restormer
./train.sh Denoising/Options/GaussianGrayDenoising_Restormer.yml

    Train Restormer for grayscale non-blind image denoising, run

cd Restormer
./train.sh Denoising/Options/GaussianGrayDenoising_RestormerSigma15.yml
./train.sh Denoising/Options/GaussianGrayDenoising_RestormerSigma25.yml
./train.sh Denoising/Options/GaussianGrayDenoising_RestormerSigma50.yml

    Train Restormer for color blind image denoising, run

cd Restormer
./train.sh Denoising/Options/GaussianColorDenoising_Restormer.yml

    Train Restormer for color non-blind image denoising, run

cd Restormer
./train.sh Denoising/Options/GaussianColorDenoising_RestormerSigma15.yml
./train.sh Denoising/Options/GaussianColorDenoising_RestormerSigma25.yml
./train.sh Denoising/Options/GaussianColorDenoising_RestormerSigma50.yml

Note: The above training scripts use 8 GPUs by default. To use any other number of GPUs, modify Restormer/train.sh and the yaml file corresponding to each task (e.g., Denoising/Options/GaussianGrayDenoising_Restormer.yml)
Evaluation

    Download the pre-trained models and place them in ./pretrained_models/

    Download testsets (Set12, BSD68, CBSD68, Kodak, McMaster, Urban100), run

python download_data.py --data test --noise gaussian

Grayscale blind image denoising testing

    To obtain denoised predictions, run

python test_gaussian_gray_denoising.py --model_type blind --sigmas 15,25,50

    To reproduce PSNR Table 4 (top super-row), run

python evaluate_gaussian_gray_denoising.py --model_type blind --sigmas 15,25,50

Grayscale non-blind image denoising testing

    To obtain denoised predictions, run

python test_gaussian_gray_denoising.py --model_type non_blind --sigmas 15,25,50

    To reproduce PSNR Table 4 (bottom super-row), run

python evaluate_gaussian_gray_denoising.py --model_type non_blind --sigmas 15,25,50

Color blind image denoising testing

    To obtain denoised predictions, run

python test_gaussian_color_denoising.py --model_type blind --sigmas 15,25,50

    To reproduce PSNR Table 5 (top super-row), run

python evaluate_gaussian_color_denoising.py --model_type blind --sigmas 15,25,50

Color non-blind image denoising testing

    To obtain denoised predictions, run

python test_gaussian_color_denoising.py --model_type non_blind --sigmas 15,25,50

    To reproduce PSNR Table 5 (bottom super-row), run

python evaluate_gaussian_color_denoising.py --model_type non_blind --sigmas 15,25,50

Real Image Denoising
Training

    Download SIDD training data, run

python download_data.py --data train --noise real

    Generate image patches from full-resolution training images, run

python generate_patches_sidd.py 

    Train Restormer

cd Restormer-main
./train.sh Denoising/Options/RealDenoising_Restormer.yml

or

cd Restormer-main
./train.sh Denoising/Options/RealDenoising_Restormer_MVTECAD.yml

Note: This training script uses 8 GPUs by default. To use any other number of GPUs, modify Restormer/train.sh and Denoising/Options/RealDenoising_Restormer.yml
Evaluation

    Download the pre-trained model and place it in ./pretrained_models/

Testing on SIDD dataset

    Download SIDD validation data, run

python download_data.py --noise real --data test --dataset SIDD

    To obtain denoised results, run

python test_real_denoising_sidd.py --save_images

    To reproduce PSNR/SSIM scores on SIDD data (Table 6), run

evaluate_sidd.m

Testing on DND dataset

    Download the DND benchmark data, run

python download_data.py --noise real --data test --dataset DND

    To obtain denoised results, run

python test_real_denoising_dnd.py --save_images

    To reproduce PSNR/SSIM scores (Table 6), upload the results to the DND benchmark website.
