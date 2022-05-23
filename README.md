# Stage 1 and 2 Recycling
## General Outline
According to Lauren Cutlip, business development and outreach manager at the Recycling & Disposal Solutions of Virginia, there are stages to the recycling process (see https://brightly.eco/recycling-process/).  
<br/>“When the driver completes their route, they arrive at the [MRF] to be weighed in on the scale before unloading the recyclable materials onto the tip floor,” Cutlip says. “Then a loader operator will scoop the materials and fill a drum feed or hopper where the items are mixed up and brought to the presort belt where staff looks for [non-recyclable] items that could damage equipment or contaminate the end product (including wood, clothing, plastic bags, scrap metal, and food waste).”
<br/>“The materials travel through a series of mechanical sorting steps, which in the case of the RDS Roanoke single-stream facility include an OCC screen where cardboard travels up onto its own conveyor, a fine screen where glass drops out, a news screen where 2D paper is separated from 3D containers, an optical sorter that pulls out PET bottles, an overhead magnet for steel and tin cans, AMP robots that pull off HDPE containers, and—finally—an eddy current that repels aluminum cans into their designated bunker.”
<br/><br/>I break this down into Stage1 and Stage 2, respectively.  The idea is to build a computer vision model for each of the stages.  Perhaps Stage 1 model could make the staff's work safer by first having robots remove all objects about which they are reasonably certain.  
<br/></br> I've scraped about half of the data using fastai's search_images function and obtained the rest from https://github.com/garythung/trashnet.  For stage 1, I've had a composite category of images from stage 2.  These items are likely to be present among mixed materials on the tip floor and thus must be flagged as items that will need to be separated during stage 2.  For more details, please see the Data Description section below.
<br/>Given my current GPU specifications, I've trained a simple Resnet34 model for each stage. The models are deployed with Gradio on Huggingface.
## Next Steps
<br/>There are at least three steps that can be taken to make the model better.
<br/><br/>As the first step, I would like to get more high quality images in each category, but especially more images to help differentiate plastic and glass bottles as well as paper and cardboard.  Right now a tilt in a bottle's image can 'fool' a model into thinking it's glass even if it's plastic and a 'straight' image of a plastic bottle can be mistaken for glass.  Some images of paper and cardboard can look remarkably similar, even to a person, and more images of the type that are likely to be confused by the model would help.  Balancing out the category counts is also something I would try if making a production-grade model.  
<br/>Secondly, I've tested the model on images it has not seen before and used those as sample images in the deployed applications.  However, a rigorous testing set for each stage is a must if it were deployed in an actual production environment.  Since this project consists of simple prototypes, I'm putting a bookmark here.  
<br/> Finally, I've trained the models locally on a 4GB GPU and settled on Resnet34.  I've tried experimenting with some of convnext or efficientnet models using the timm package (see https://www.kaggle.com/code/jhoward/which-image-models-are-best/), but I would have liked to experiment more using a better GPU in the future.  This would be less important than the previous two steps, however.   
<br/><br/> The models are deployed with Gradio on Huggingface at https://huggingface.co/spaces/dpv/Stage1Recycling and https://huggingface.co/spaces/dpv/Stage2Recycling

## Data Description
<br/>
Stage 1 counts are as follows: 
    <br/>discarded clothing (143)
    <br/>food waste(184)
    <br/>plastic bags (283)
    <br/>scrap metal pieces (123)
    <br/>wood scraps (288)
    <br/>recyclables no scrap (492 images from stage 2 recyclables, roughly the same amount from each category in stage 2)
<br/><br/>
Stage 2 counts are as follows: 
    <br/>aluminium can (247)
    <br/>cardboard(403) <-From Trasnhet
    <br/>glass (501) <-From Trasnhet
    <br/>HDPE container (318)
    <br/>2D paper (594) <-From Trasnhet
    <br/>3D paper (222)
    <br/>PET plastic bottle (274) <-From Trasnhet
    <br/>steel and tin cans (250)
