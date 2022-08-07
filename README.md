# C#/.NET Live Project

## Introduction
During a two-week internship at Prosper IT Consulting, I worked on a team with other software development students to create an ASP.NET MVC web application using Entity Framework for data access. The web application was designed for a theater company to manage its website without requiring any technical knowledge. The application has multiple areas to manage admin needs, subscriber needs, and general public needs. The site includes information on the current season, past productions, and the current cast members. Below are descriptions of the stories I worked on along with code snippets.

## Stories
* [Style Landing Page](#style-landing-page)
* [Create a Photo Class](#create-a-photo-class)
* [Style Create Page](#style-create-page)
* [Photo Storage Retrieval](#photo-storage-retrieval)
* [Style Index Page](#style-index-page)

## Style Landing Page
I was tasked with styling the Landing Page that would meet the criteria of the client. It was requested that under the NavBar would be the Theatre Logo. A section called Spotlight that would display the upcoming events, the current production posters were provided. There would also be sections for both Donations and Sponsors.  The end of the page includes a section called Land Agreement.

[Home Page CSS](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/CSS/HomeCSS.txt)

[Home Page Code](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/Page%20Code/HomePageCode.txt)

![Home Page](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/Images/HomePage.png)



## Create a Photo Class:

I was tasked with creating a photo class entity model and scaffolding the Crud pages associated with this model. 
```
    public class ProdPhoto 
    {
        [Key]
        public int ProPhotoId { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
    }
 ```
 
 
## Style Create Page:

I was tasked to style the Production Photo Create CRUD page. I completed what was requested of me in quick and efficient fashion. The specific requests included a header called "Create Production Photo". Styling for both buttons + added hover effects. Included an image placeholder and provided a way for the image to visible after selecting your chosen file.

[Create Page CSS](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/CSS/CreateCSS.txt)

[Create Page Code](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/Page%20Code/CreatePageCode.txt)

![Create Page](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/Images/CreatePage.png)

## Photo Storage Retrieval:

I was tasked with creating an entity model for the Production Photo class that has all the methods and helpers to improve consistency, reduce complexity, and reduce calls to the database.
First, I created the Photo class model with the following attributes:
```
    public class ProdPhoto 
    {
        [Key]
        public int ProPhotoId { get; set; }
        public Byte[] PhotoFile { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
    }
 ```
Once the new class was created, I added to an existing DbSet the, and created a new controller with scaffolded views based on the Photo class. I added code to the Photo controller for uploading an image:
```
        //Convert image to byte 

        public byte[] convertImage(HttpPostedFileBase postedFile)
        {
            byte[] bytes;
            using (BinaryReader br = new BinaryReader(postedFile.InputStream))
            {
                bytes = br.ReadBytes(postedFile.ContentLength);
            }
            return bytes;
        }
```
I then implemented in into the Create Method:
```
[HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ProPhotoId,PhotoFile,Title,Description")] ProdPhoto prodPhoto, HttpPostedFileBase postedFile)
        {
            if (ModelState.IsValid)
            {
                prodPhoto.PhotoFile = convertImage(postedFile);

                db.ProdPhotos.Add(prodPhoto);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View(prodPhoto);
        }
```

## Style Index Page:

I was tasked with Styling the Index page. This included, creating a new image button, and creating bootstrap cards for each image in the database. Every card was to have a clickable imnage that would direct to the productions details page. I was also asked to organize the photos by their production name as shown below.

[Index Page CSS](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/CSS/IndexCSS.txt)

[Index Page Code](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/Page%20Code/IndexPageCode.txt)

```
    public class ProdPhoto 
    {
        [Key]
        public int ProPhotoId { get; set; }
        public Byte[] PhotoFile { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
        public string Production { get; set; }
    }
```

![Index Page](https://github.com/H-Grayson/CodeSummary_LiveProject/blob/main/Images/IndexPage.png)


## Conclusion
This live project was an invaluable opportunity to really test myself and get a glimpse of what an Agile work environment looks like. I gained experience with ASP .NET MVC, creating a code first model web application and subsequent views with CRUD functionality via the controller. I learned more about utilizing bootstrap in my project to ensure proper resposiveness. I also learned the importance of naming conventions when working on a large group project. Working with a group of developers over the course of project and being able to have daily standups in which we discussed the previous days work, roadblocks/breakthroughs, current workflow and general questions was a great way to communicate and learn from my fellow students. Overall, I really relished the real world experience that this Live Project provided me and look forward to applying what I have learned in the future.
