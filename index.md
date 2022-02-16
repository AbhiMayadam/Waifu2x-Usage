<head>
	<style>
	p{
			font-size: 20px;
		 }
	li{
			font-size: 20px;
		 }
	</style>
</head>

# How do I get and use Waifu2x?

There is 3 different versions
1. [Waifu2x-ncnn-vulkan](https://github.com/f11894/waifu2x-ncnn-vulkan-GUI/releases)
2. [Waifu2x-caffe](https://github.com/lltcggie/waifu2x-caffe/releases)
3. [Waifu2x-Colab](https://colab.research.google.com/drive/1RjyCk30cc24ez1-a1Qa3CP3g_yk9AJwq) (There is a guide in the link itself, but I will make a more detailed guide eventually)

Waifu2x-ncnn-vulkan will run on basically any hardware from 2014 and later, waifu2x-caffe will run only on Nvidia GPUs (and CPU, but that's slow as balls, so don't even think about it) and Waifu2x-Colab is an implementation of Waifu2x-ncnn vulkan that runs on Google's servers and is based around your Google Drive, and this only needs access to a web browser.

## How to check what GPU do you have?
1. Right click on the taskbar and launch Task Manager and click on more details if your Task Manager looks like this.
![TM](https://i.imgur.com/uqQgAoP.png)
2. Go to the performance tab and scroll the left pane all the way down and you will see GPU entries.
![TM](https://i.imgur.com/3ejEpOM.png)
3. Select GPU 0 or GPU 1 if your system has 2 GPUs
![TM](https://i.imgur.com/hrrGuSN.png)
4. If the GPU brand is AMD or Intel, you need to use Waifu2x-ncnn-vulkan
4a. If you have an Nvidia GPU, look at the Dedicated GPU Memory Entry. If it says 2 GB or less like here, use Waifu2x-ncnn-vulkan, if it is greater than 2 GB, use Waifu2x-caffe.

![TM](https://i.imgur.com/Wbdm50B.png)


## Using Waifu2x
1. Unzip Waifu2x-ncnn-vulkan-gui or Waifu2x-Caffe into the folder,
2. Click on waifu2x-ncnn-vulkan-gui.exe or waifu2x-caffe.exe depending on the version you downloaded.
<figure>
<img align="middle" src="https://i.imgur.com/LkZIusX.png" alt="Waifu2x NCNN Vulkan Explorer Screenshot" style="width:100%" />
<figcaption align = "center"><b>Waifu2x NCNN-Vulkan GUI</b></figcaption>
</figure>

<figure>
<img align="middle" src="https://i.imgur.com/DAKSskp.png" alt="Waifu2x NCNN Caffe Explorer Screenshot" style="width:90%" align="middle" />
<figcaption align = "center"><b>Waifu2x Caffe</b></figcaption>
</figure>

3. If you are using Waifu2x-NCNN-Vulkan, make sure that the GPU ID is set correctly for your desired GPU. If you are using just the iGPU, set it to 0. If you are using your dGPU, set it to 1. If you are not sure, you can check task manager and look at the GPU entries. It will tell you what GPU is GPU 0 and what is GPU 1.

![w2x](https://i.imgur.com/ocJQbP0.jpg)

4. Go to Preferences and enable Execute high precision processing (fp32 mode). This will increase processing time, but remove the color cast that arises from fp16 mode.
![w2x](https://media.discordapp.net/attachments/722854123434803286/943467728273047592/unknown.png)

5. For Waifu2x-Caffe, click on the App Setting button and make sure it is set to CUDA. Unlike Waifu2x-ncnn-vulkan, the GPU ID doesn't need to be set to 1.
![w2x](https://i.imgur.com/vvJQADl.png)

6. From here on, the instructions will be basically the same for either Waifu2x version. Click on Input Directory/Source for caffe and vulkan respectively, and navigate to the images you want. Caffe supports folders (but not folders within folders) while vulkan needs all the images highlighted.

7. Select your output directory by clicking on either Browse/Destination (Caffe/Vulkan). The parent directory will not be made so just make a blank folder for each chapter.

8. Make sure output is set to PNG and not JPG.

9. Denoising levels is based on how many artifacts you see. You can use an jpg quality checker website such as <https://www.imgonline.com.ua/eng/determine-jpeg-quality.php>. Upload an unstitched raw image (must be jpg originally, do not convert) and get the value.

10. Set it to Denoise and set your denoise level based on this number. Level 0 is 93+, Level 1 is 75-90, Level 2 is 75 and below, but might as well use Level 3 at this point. Do not over denoise as an over denoised image will look worse.

11. Alternatively, you can determine what level is needed if you zoom all the way in on a border of a line. If the colors bordering the line are solid, use Level 0, if there is some artifacts (small splotches that are lighter or darker than the base color), use level 1. Use Level 3 if there is a lot of them (and I mean a lot)

12. Make sure the box for TTA is not checked. It will process each image 8 times in different orientations and average the differences to get the best result, but obviously increases processing time by 8x. TTA doesn't make enough of a difference to warrant it for Webtoons.

13. Click Run and wait for it to process.

## FAQ
**My Task Manager looks like this and has no GPU entries?**

![fuckywucky](https://i.imgur.com/BwrHd7x.png)

You will need to install your GPU drivers or you have a very old Graphics Card. If it is the latter, use Device Manager, then select Display Adapters to determine what GPU you have. Odds are if it doesn't show up in Task Manager, there is a non zero chance that it doesn't support Vulkan and either waifu2x-caffe (If you have Nvidia) or waifu2x-colab must be used.

**I am running Waifu2x and I do not see any GPU activity?**

For some reason, Windows does not report GPU Compute at all. Go to your GPU entry in Task Manager, click on any of the titles of the graphs (Don't choose 3D processing or Copy) and select Compute 0. Waifu2x activity will show up. You will also see spikes under the Copy Graph.
![fuckywucky](https://i.imgur.com/ZNHQYMf.png)

**I got an error that looked something like this and Waifu2x-ncnn-vulkan stopped processing?**

![fuckywucky](https://i.imgur.com/qTWsZfJ.png)

What happened was that you ran out of VRAM and Waifu2x couldn't use system memory. This occurs when you have slow video memory relative to system memory. My laptop has GDDR3 video memory and DDR4 system memory (Thanks AMD) and due to DDR4 being much faster than GDDR3, it will throw this issue. In this case, you need to reduce the block size to something reasonable. Block size isn't a truly set and forget setting. It depends on the physical size of your raws, how many threads you are procesisng at once (2 is the default), and how much VRAM you have. Try to find a setting that uses 1/3 to 1/2 of your VRAM. 200 to 500 is an ideal block size. If you have a lot of VRAM, you can increase it higher.
If you are working with Raws that are like 20000 pixels tall, please slice it into a sane size like 10000 pixel tall. Your computer will thank you, and your CL, TS, RD, and QC's computers will thank you.

**I got an error like this and Waifu2x-ncnn-vulkan didn't even launch?**

![fuckywucky](https://i.imgur.com/RrjPBjK.png)

Your GPU doesn't support Vulkan. If you have another GPU, switch to that (even if it is an iGPU) and make sure your GPU drivers are up to date. If you have done all you can, you will need to use Waifu2x-Colab for denoising.
