Github Link: https://github.com/st10114295/Farm_Central/releases/tag/_st10114295_ProgPart2_FarmCentral

Name: Nicole Willemse
Student Number: ST10114295

Programming 3A

AIM:
To develop a prototype web application using Visual Studio and C#. In addition, your prototype must
have the following functionality:
 The prototype website shall have a database of farmers with their associated products.
 The prototype website shall provide two different user roles: farmer and employee.
 The prototype website shall require farmers and employees to log into the website to
access user‐specific information.
 The prototype website shall allow a logged‐in employee to add a new farmer to the
database.
 The prototype website shall allow a logged‐in farmer to add a new product to their profile
in the database.
 The prototype website shall allow a logged‐in employee to view the list of all the products
ever supplied by a specific farmer.
 The prototype website shall allow a logged‐in employee to filter the displayed list of
products supplied by a specific farmer according to the date range or product type.

HOW TO COMPILE:
1. The Required Packages:
   Install-Package Microsoft.EntityFrameworkCore
   Install-Package Microsoft.EntityFrameworkCore.SqlServer  
   Install-Package Microsoft.EntityFrameworkCore.Tools
   Install-Package Bootstrap
2. To start compiling the application, I installed Visual Studio 2022 and  
   Microsoft SQL     
   Management Server.
3. Click on 'Create New Project' and select 'ASP.NET Core Web App (Model-
   View-Controller).
4. Next, you'll need to enter a project name, and then select the .NET  
   (Standard Term Support).
5. Configure for HTTPS, and click on 'Create'.

THE CODE THAT I'VE USED:
1. For the Authentication:
   In the Areas/Identity/Data Folder, create two classes named  
   "ApplicationUser", and "AuthDBContext" :

   using System;
   using System.Collections.Generic;
   using System.ComponentModel.DataAnnotations.Schema;
   using System.Linq;
   using System.Threading.Tasks;
   using Microsoft.AspNetCore.Identity;

namespace Farm_Central.Areas.Identity.Data;

// Add profile data for application users by adding properties to the ApplicationUser class
public class ApplicationUser : IdentityUser
{
    [PersonalData]
    [Column(TypeName ="nvarchar(100)")]
    public string FirstName { get; set; }

    [PersonalData]
    [Column(TypeName = "nvarchar(100)")]
    public string LastName { get; set; }
}
**************************************************************************

using Farm_Central.Areas.Identity.Data;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

namespace Farm_Central.Data;

public class AuthDBContext : IdentityDbContext<ApplicationUser>
{
    public AuthDBContext(DbContextOptions<AuthDBContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        // Customize the ASP.NET Identity model and override the defaults if needed.
        // For example, you can rename the ASP.NET Identity table names and more.
        // Add your customizations after calling base.OnModelCreating(builder);
    }
}
**************************************************************************

2. In the Areas/Identity/Pages/Account Folder:

@page
@model LoginModel

@{
    ViewData["Title"] = "Log in";
    Layout = "~/Areas/Identity/Pages/_AuthLayout.cshtml";
}


<div class="row justify-content-center">
    <div class="col-10">
        <section>
            <form id="account" method="post">
                <h2>Welcome Back!</h2>
                <hr />
                <div asp-validation-summary="ModelOnly" class="text-danger" role="alert"></div>
                <div class="form-floating mb-3">
                    <input asp-for="Input.Email" class="form-control" autocomplete="username" aria-required="true" placeholder="name@example.com" />
                    <label asp-for="Input.Email" class="form-label">Email</label>
                    <span asp-validation-for="Input.Email" class="text-danger"></span>
                </div>
                <div class="form-floating mb-3">
                    <input asp-for="Input.Password" class="form-control" autocomplete="current-password" aria-required="true" placeholder="password" />
                    <label asp-for="Input.Password" class="form-label">Password</label>
                    <span asp-validation-for="Input.Password" class="text-danger"></span>
                </div>
                <div class="checkbox mb-3">
                    <label asp-for="Input.RememberMe" class="form-label">
                        <input class="form-check-input" asp-for="Input.RememberMe" />
                        @Html.DisplayNameFor(m => m.Input.RememberMe)
                    </label>
                </div>
                <div>
                    <button id="login-submit" type="submit" class="w-100 btn btn-lg btn-primary">Log in</button>
                </div>
            </form>
        </section>
    </div>
 
</div>

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
***************************************************************************
@page
@model RegisterModel
@{
    ViewData["Title"] = "Register";
    Layout = "~/Areas/Identity/Pages/_AuthLayout.cshtml";
}




        <form id="registerForm" asp-route-returnUrl="@Model.ReturnUrl" method="post">
            <h2>Create a new account.</h2>
            <hr />
            <div asp-validation-summary="ModelOnly" class="text-danger" role="alert"></div>
            <div class="row">
                <div class="col-6">
                    <div class="form-floating mb-3">
                <input asp-for="Input.FirstName" class="form-control" autocomplete="firstname" aria-required="true" placeholder="first name" />
                        <label asp-for="Input.FirstName">First Name</label>
                    </div>
                </div>
                 <div class="col-6">
                     <div class="form-floating mb-3">
                <input asp-for="Input.LastName" class="form-control" autocomplete="lastname" aria-required="true" placeholder="last name" />
                        <label asp-for="Input.LastName">Last Name</label>
                    </div>
                 </div>
                 </div>
           
            <div class="form-floating mb-3">
                <input asp-for="Input.Email" class="form-control" autocomplete="email" aria-required="true" placeholder="email" />
                <label asp-for="Input.Email">Email</label>
                <span asp-validation-for="Input.Email" class="text-danger"></span>
            </div>
            <div class="form-floating mb-3">
                <input asp-for="Input.Password" class="form-control" autocomplete="new-password" aria-required="true" placeholder="password" />
                <label asp-for="Input.Password">Password</label>
                <span asp-validation-for="Input.Password" class="text-danger"></span>
            </div>
            <div class="form-floating mb-3">
                <input asp-for="Input.ConfirmPassword" class="form-control" autocomplete="new-password" aria-required="true" placeholder="password" />
                <label asp-for="Input.ConfirmPassword">Confirm Password</label>
                <span asp-validation-for="Input.ConfirmPassword" class="text-danger"></span>
            </div>
            <button id="registerSubmit" type="submit" class="w-100 btn btn-lg btn-primary">Register</button>
        </form>
   

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
**************************************************************************

3. In the Areas/Identity/Pages Folder:
@{
    Layout = "/Views/Shared/_Layout.cshtml";
}
<div class="row justify-content-center">
    <div class="col-5 mt-5">
        <div class="card login-logout-card">
            <div class="card-header">
                <ul class="nav nav-tabs card-header-tabs">
                    <li class="nav-item">
                        <a class="nav-link" href="/Identity/Account/Login">
                        Login
                    </a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="/Identity/Account/Register"
                        Register
                        ></a>
                    </li>
                </ul>
            </div>
            <div class="card-body">
                <div class="row justify-content-center">
                    <div class="col-10 my-4">
                        @RenderBody()
                    </div>
                </div>
            </div>
         </div>
    </div>
</div>

@section scripts{
    @await RenderSectionAsync("Scripts", required: false)
    <script>
        $(function () {
            const currentPage = location.pathname;
            $('.nav-tabs li a').each(function () {
                const $this =$(this);
                if (currentPage.toLowerCase().indexOf($this.attr('href').toLowerCase()) ≢ -1  {
                    $this.addClass('active')
                }
            }
        })
    </script>
}
***************************************************************************
<environment include="Development">
    <script src="~/Identity/lib/jquery-validation/dist/jquery.validate.js"></script>
    <script src="~/Identity/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.js"></script>
</environment>
<environment exclude="Development">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.17.0/jquery.validate.min.js"
            asp-fallback-src="~/Identity/lib/jquery-validation/dist/jquery.validate.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.validator"
            crossorigin="anonymous"
            integrity="sha384-rZfj/ogBloos6wzLGpPkkOr/gpkBNLZ6b6yLy4o+ok+t/SAKlL5mvXLr0OXNi1Hp">
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validation-unobtrusive/3.2.11/jquery.validate.unobtrusive.min.js"
            asp-fallback-src="~/Identity/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.validator && window.jQuery.validator.unobtrusive"
            crossorigin="anonymous"
            integrity="sha384-R3vNCHsZ+A2Lo3d5A6XNP7fdQkeswQWTIPfiYwSpEP3YV079R+93YzTeZRah7f/F">
    </script>
</environment>
***************************************************************************
4. In the Controllers folder, create three classes named "FarmerController", "HomeController", and "ProductController" :

using Farm_Central.DataAccessLayer;
using Farm_Central.Models;
using Farm_Central.Models.DBEntities;
using Microsoft.AspNetCore.Mvc;

namespace Farm_Central.Controllers
{
    public class FarmerController : Controller
    {
        private readonly FarmerDbContext _context;

        public FarmerController(FarmerDbContext context)
        {
            this._context = context;
        }

        [HttpGet]
        public IActionResult Index()
        {
            var farmers = _context.Farmers.ToList();
            List<FarmerViewModel> farmerList = new List<FarmerViewModel>();

            if (farmers != null)
            {
                foreach (var farmer in farmers)
                {
                    var FarmerViewModel = new FarmerViewModel()
                    {
                        Id = farmer.Id,
                        FullName = farmer.FullName,
                        City = farmer.City,
                        ContactNum = farmer.ContactNum,
                    };
                    farmerList.Add(FarmerViewModel);
                }

                return View(farmerList);
            }
            return View(farmerList);
        }
        [HttpGet]
        public IActionResult Create()
        {
            return View();
        }

        [HttpPost]
        public IActionResult Create(FarmerViewModel farmerData)
        {
            try
            {
                if (ModelState.IsValid)
                {
                    var farmer = new Farmer()
                    {
                        FullName = farmerData.FullName,
                        City = farmerData.City,
                        ContactNum = farmerData.ContactNum
                    };
                    _context.Farmers.Add(farmer);
                    _context.SaveChanges();
                    TempData["SuccessMessage"] = "Farmer Added Successfully!";
                    return RedirectToAction("Index");
                }
                else
                {
                    TempData["errorMessage"] = "Data Is Not Valid!";
                    return View();
                }
            }
            catch (Exception ex)
            {
                TempData["errorMessage"] = ex.Message;
                return View();
            }
        }
        [HttpGet]
        public IActionResult Edit(int Id)
        {
            try
            {
                var farmer = _context.Farmers.SingleOrDefault(x => x.Id == Id);

                if (farmer != null)
                {
                    var farmerView = new FarmerViewModel()
                    {
                        Id = farmer.Id,
                        FullName = farmer.FullName,
                        City = farmer.City,
                        ContactNum = farmer.ContactNum
                    };


                    return View(farmerView);
                }
                else
                {
                    TempData["errorMessage"] = $"There Are No Details Available With The Id: {Id}";
                    return RedirectToAction("Index");
                }
            }
            catch (Exception ex)
            {
                TempData["errorMessage"] = ex.Message;
                return RedirectToAction("Index");
            }
        }
        [HttpPost]
        public IActionResult Edit(FarmerViewModel model)
        {
            try
            {
                if (ModelState.IsValid)
                {
                    var farmer = new Farmer()
                    {
                        Id = model.Id,
                        FullName = model.FullName,
                        City = model.City,
                        ContactNum = model.ContactNum
                    };
                    _context.Farmers.Update(farmer);
                    _context.SaveChanges();
                    TempData["successMessage"] = "Farmer Details Updated Successfully!";
                    return RedirectToAction("Index");
                }
                else
                {
                    TempData["errorMessage"] = "Data Is Invalid!";
                    return View();
                }
            }
            catch (Exception ex)
            {

                TempData["errorMessage"] = ex.Message;
                return View();
            }
            
        }

        [HttpGet]
        public IActionResult Delete(int Id)
        {
            try
            {
                var farmer = _context.Farmers.SingleOrDefault(x => x.Id == Id);

                if (farmer != null)
                {
                    var farmerView = new FarmerViewModel()
                    {
                        Id = farmer.Id,
                        FullName = farmer.FullName,
                        City = farmer.City,
                        ContactNum = farmer.ContactNum
                    };


                    return View(farmerView);
                }
                else
                {
                    TempData["errorMessage"] = $"There Are No Details Available With The Id: {Id}";
                    return RedirectToAction("Index");
                }
            }
            catch (Exception ex)
            {
                TempData["errorMessage"] = ex.Message;
                return RedirectToAction("Index");
            }
        }

        [HttpPost]
        public IActionResult Delete(FarmerViewModel model)
        {
            try
            {
                var farmer = _context.Farmers.SingleOrDefault(x => x.Id == model.Id);

                if (farmer != null)
                {
                    _context.Farmers.Remove(farmer);
                    _context.SaveChanges();
                    TempData["successMessage"] = "Farmer Details Deleted Successfully!";
                    return RedirectToAction("Index");
                }
                else
                {
                    TempData["errorMessage"] = $"There Are No Details Available With The Id: {model.Id}";
                    return RedirectToAction("Index");
                }
            }
            catch (Exception ex)
            {

                TempData["errorMessage"] = ex.Message;
                return View();
            }
        }
    }
}
   
***************************************************************************
using Farm_Central.Areas.Identity.Data;
using Farm_Central.Models;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Mvc;
using System.Diagnostics;

namespace Farm_Central.Controllers
{

    [Authorize]
    public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;
        private readonly UserManager<ApplicationUser> _userManager;

        public HomeController(ILogger<HomeController> logger, UserManager<ApplicationUser> userManager)
        {
            _logger = logger;
            this._userManager = userManager;
        }

        [HttpGet]
        public IActionResult Index()
        {
            {
                ViewData["UserID"] = _userManager.GetUserId(this.User);
                return View();
            }
        }

       
    

public IActionResult Privacy()
        {
            return View();
        }

        [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
        public IActionResult Error()
        {
            return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
        }
    }
}
**************************************************************************
using Farm_Central.DataAccessLayer;
using Farm_Central.Models;
using Farm_Central.Models.DBEntities;
using Microsoft.AspNetCore.Mvc;

namespace Farm_Central.Controllers
{
    public class ProductController : Controller
    {
        private readonly ProductDbContext _context;

        public ProductController(ProductDbContext context)
        {
            this._context = context;
        }

        [HttpGet]
        public IActionResult Index()
        {
            var products = _context.Products.ToList();
            List<ProductViewModel> productList = new List<ProductViewModel>();
            if (products != null)
            {
                foreach (var product in products)
                {
                    var ProductViewModel = new ProductViewModel()
                    {
                        ProductId = product.ProductId,
                        FarmerName = product.FarmerName,
                        ProductName = product.ProductName,
                        ProductDescription = product.ProductDescription,
                        ProductPrice = product.ProductPrice,
                        DateManufactured = product.DateManufactured,
                    };
                    productList.Add(ProductViewModel);
                }

                return View(productList);
            }
            return View(productList);
        }
        [HttpGet]
        public IActionResult Create()
        {
            return View();
        }

        [HttpPost]
        public IActionResult Create(ProductViewModel productData)
        {
            try
            {
                if (ModelState.IsValid)
                {
                    var product = new Product()
                    {
                        FarmerName = productData.FarmerName,
                        ProductName = productData.ProductName,
                        ProductDescription = productData.ProductDescription,
                        ProductPrice = productData.ProductPrice,
                        DateManufactured = productData.DateManufactured

                    };
                    _context.Products.Add(product);
                    _context.SaveChanges();
                    TempData["SuccessMessage"] = "Product Added Successfully!";
                    return RedirectToAction("Index");
                }
                else
                {
                    TempData["errorMessage"] = "Data Is Not Valid!";
                    return View();
                }
            }
            catch (Exception ex)
            {
                TempData["errorMessage"] = ex.Message;
                return View();
            }
        }

        [HttpGet]
        public IActionResult Edit(int ProductId)
        {
            try
            {
                var product = _context.Products.SingleOrDefault(x => x.ProductId == ProductId);

                if (product != null)
                {
                    var productView = new ProductViewModel()
                    {
                        ProductId = product.ProductId,
                        ProductName = product.ProductName,
                        ProductDescription = product.ProductDescription,
                        ProductPrice = product.ProductPrice,
                        DateManufactured = product.DateManufactured

                    };


                    return View(productView);
                }
                else
                {
                    TempData["errorMessage"] = $"There Are No Details Available With The Id: {ProductId}";
                    return RedirectToAction("Index");
                }
            }
            catch (Exception ex)
            {

                TempData["errorMessage"] = ex.Message;
                return RedirectToAction("Index");
            }
        }

        [HttpPost]
        public IActionResult Edit(ProductViewModel model)
        {
            try
            {
                if (ModelState.IsValid)
                {
                    var product = new Product()
                    {
                        ProductId = model.ProductId,
                        ProductName = model.ProductName,
                        ProductDescription = model.ProductDescription,
                        ProductPrice = model.ProductPrice,
                        DateManufactured = model.DateManufactured,
                        FarmerName = model.FarmerName

                    };
                    _context.Products.Update(product);
                    _context.SaveChanges();
                    TempData["successMessage"] = " Details Updated Successfully!";
                    return RedirectToAction("Index");
                }
                else
                {
                    TempData["errorMessage"] = "Data Is Invalid!";
                    return View();
                }
            }
            catch (Exception ex)
            {

                TempData["errorMessage"] = ex.Message;
                return View();
            }
        }

        [HttpGet]
        public IActionResult Delete(int ProductId)
        {
            try
            {
                var product = _context.Products.SingleOrDefault(x => x.ProductId == ProductId);

                if (product != null)
                {
                    var productView = new ProductViewModel()
                    {
                        ProductId = product.ProductId,
                        ProductName = product.ProductName,
                        ProductDescription = product.ProductDescription,
                        ProductPrice = product.ProductPrice,
                        DateManufactured = product.DateManufactured

                    };


                    return View(productView);
                }
                else
                {
                    TempData["errorMessage"] = $"There Are No Details Available With The Id: {ProductId}";
                    return RedirectToAction("Index");
                }
            }
            catch (Exception ex)
            {

                TempData["errorMessage"] = ex.Message;
                return RedirectToAction("Index");
            }
        }
        [HttpPost]
        public IActionResult Delete(ProductViewModel model)
        {
            
            {
                try
                {
                    var product = _context.Products.SingleOrDefault(x => x.ProductId == model.ProductId);

                    if (product != null)
                    {
                        _context.Products.Remove(product);
                        _context.SaveChanges();
                        TempData["successMessage"] = "Product Details Deleted Successfully!";
                        return RedirectToAction("Index");
                    }
                    else
                    {
                        TempData["errorMessage"] = $"There Are No Details Available With The Id: {model.ProductId}";
                        return RedirectToAction("Index");
                    }
                }
                catch (Exception ex)
                {

                    TempData["errorMessage"] = ex.Message;
                    return View(model);
                }
            }
        }
        }
    }
***************************************************************************
5. Create a new folder called "DataAccessLayer.
   Create two classes named, "FarmerDbContext", and "ProductDbContext" :

using Farm_Central.Models.DBEntities;
using Microsoft.EntityFrameworkCore;

namespace Farm_Central.DataAccessLayer
{
    public class FarmerDbContext :DbContext
    {
        public FarmerDbContext(DbContextOptions<FarmerDbContext> options) 
            : base(options)
        {
        }

        public DbSet<Farmer> Farmers { get; set; }
    }
}
***************************************************************************
using Farm_Central.Models.DBEntities;
using Microsoft.EntityFrameworkCore;

namespace Farm_Central.DataAccessLayer
{
    public class ProductDbContext : DbContext
    {
        public ProductDbContext(DbContextOptions options) : base(options)
        {
        }

        public DbSet<Product> Products { get; set; }
    }
}
****************************************************************************
6. In Models, create three classes, named "ErrorViewModel", 
   "FarmerViewModel", and "ProductViewModel"

namespace Farm_Central.Models
{
    public class ErrorViewModel
    {
        public string? RequestId { get; set; }

        public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);
    }
}
          ************************************************
namespace Farm_Central.Models
{
    public class ErrorViewModel
    {
        public string? RequestId { get; set; }

        public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);
    }
}
                           ************************************************

using Farm_Central.Models.DBEntities;
using Microsoft.EntityFrameworkCore.Metadata.Internal;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations.Schema;

namespace Farm_Central.Models
{
    public class ProductViewModel
    {
        public int ProductId { get; set; }
        [DisplayName("Product Name")]
        public string ProductName { get; set; }

        [DisplayName("Product Description")]
        public string ProductDescription { get; set; }

        [DisplayName("Price of The Product")]
        public double ProductPrice { get; set; }

        [DisplayName("Manufactured Date")]
        public DateTime DateManufactured { get; set; }

        [DisplayName("Farmers Full Name")]
        public string FarmerName { get; set; }
    }
}
*************************************************************************
7. In Models, create a new folder named "DBEntities".
   Create two classes inside DBEntities, named "Farmer", and "Product"

using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace Farm_Central.Models.DBEntities
{
    public class Farmer
    {
        [Key]
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }
        [Column(TypeName ="varchar(100)")]
        public string FullName { get; set; }
        [Column(TypeName = "varchar(50)")]
        public string City { get; set; }
        [Column(TypeName = "varchar(10)")]
        public string ContactNum { get; set; }
        

    }
}
                    *************************************

using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace Farm_Central.Models.DBEntities
{
    public class Farmer
    {
        [Key]
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }
        [Column(TypeName ="varchar(100)")]
        public string FullName { get; set; }
        [Column(TypeName = "varchar(50)")]
        public string City { get; set; }
        [Column(TypeName = "varchar(10)")]
        public string ContactNum { get; set; }
        

    }
}

Create  migrations and update your database 
**************************************************************************
8. In Views/Farmer:

@model Farm_Central.Models.FarmerViewModel
    
@{
    ViewData["Title"] = "Add Farmer";
    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr/>
@if (!string.IsNullOrWhiteSpace(errorMessage))
{
<div class="alert alert-danger">
    <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
</div>

}
<form action="Create" method="post" autocomplete="off">
    <div class="mb-3">
        <label class="form-label">Full Name</label>
        <input type="text" class="form-control" asp-for="FullName" />
        <span asp-validation-for="FullName" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">City</label>
        <input type="text" class="form-control" asp-for="City" />
        <span asp-validation-for="City" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Contact Number</label>
        <input type="text" class="form-control" asp-for="ContactNum" />
        <span asp-validation-for="ContactNum" class="text-danger"></span>
    </div>

    <div class="d-grid">
        <button type="submit" class="btn btn-primary">Submit</button>
    </div>
    
</form>
@section Scripts{
    @{
        await Html.RenderPartialAsync("_ValidationScriptsPartial");
    }
}
                    *******************************************
@model Farm_Central.Models.FarmerViewModel


@{
    ViewData["Title"] = "Delete Farmer Details";

    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr />
@if (!string.IsNullOrWhiteSpace(errorMessage))
{
    <div class="alert alert-danger">
        <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>

}
<form action="Delete" method="post" autocomplete="off">
    <div class="mb-3">
        <label class="form-label">Id</label>
        <input type="text" class="form-control" asp-for="Id" readonly />
    </div>

    <div class="mb-3">
        <label class="form-label">Full Name</label>
        <input type="text" class="form-control" asp-for="FullName" readonly/>
    </div>

    <div class="mb-3">
        <label class="form-label">City</label>
        <input type="text" class="form-control" asp-for="City" readonly/>
    </div>

    <div class="mb-3">
        <label class="form-label">Contact Number</label>
        <input type="text" class="form-control" asp-for="ContactNum" readonly/>
    </div>
    <div class="d-grid">
        <button type="submit" class="btn btn-danger" onclick="return confirm('Are you sure you want to delete farmer : @Model.FullName')? ">Delete</button>
    </div>
</form>
                    *****************************************

@model Farm_Central.Models.FarmerViewModel


@{
    ViewData["Title"] = "Edit Farmer's Details";

    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr />
@if (!string.IsNullOrWhiteSpace(errorMessage))
{
    <div class="alert alert-danger">
        <strong>Error! </strong>@errorMessage
            <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>

}
<form action="Edit" method="post" autocomplete="off">
    <div class="mb-3">
        <label class="form-label">Id</label>
        <input type="text" class="form-control" asp-for="Id" readonly />
    </div>

    <div class="mb-3">
        <label class="form-label">Full Name</label>
        <input type="text" class="form-control" asp-for="FullName" />
        <span asp-validation-for="FullName" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">City</label>
        <input type="text" class="form-control" asp-for="City" />
        <span asp-validation-for="City" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Contact Number</label>
        <input type="text" class="form-control" asp-for="ContactNum" />
        <span asp-validation-for="ContactNum" class="text-danger"></span>
    </div>
    <div class="d-grid">
        <button type="submit" class="btn btn-success">Update</button>
    </div>
</form>
@section Scripts{
    @{
        await Html.RenderPartialAsync("_ValidationScriptsPartial");
    }
}
             ********************************************
@model List<Farm_Central.Models.FarmerViewModel>
    
@{
    ViewData["Title"] = "View Farmers";
    var successMessage = TempData["successMessage"]?.ToString();
    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr />
@if (!string.IsNullOrWhiteSpace(successMessage))
{
    <div class="alert alert-success">
        <strong>Success! </strong>@successMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>
    
}
else if (!string.IsNullOrWhiteSpace(errorMessage))
{
    <div class="alert alert-danger">
        <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>
   
}
<form>
    <button asp-action="Create" asp-controller="Farmer" class="btn btn-primary mb-3">Add A New Farmer</button>
    <table class="table table-responsive table-hover table-bordered">
        <thead>
            <tr class="table-active">
                <th>Id</th>
                <th>Full</th>
                <th>City</th>
                <th>Contact Number</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            @if (Model != null && Model.Any())
            {
                @foreach (var farmer in Model)
                {
                    <tr>
                        <td>@farmer.Id</td>
                        <td>@farmer.FullName</td>
                        <td>@farmer.City</td>
                        <td>@farmer.ContactNum</td>
                        <td>
                            <div class="btn-group btn-group-sm">
                                <a asp-controller="Farmer" asp-action="Edit" asp-route-id="@farmer.Id" class="btn btn-primary">Edit</a>
                                
                                <a asp-controller="Farmer" asp-action="Delete" asp-route-id="@farmer.Id" class="btn btn-danger">Delete</a>
                            </div>
                        </td>
                    </tr>
                }
            }
            else
            {
                <tr>
                    <td colspan="6">
                        <div>
                            No Farmers Have Been Captured In The Database!
                        </div>
                    </td>
                </tr>
                
            }
        </tbody>
    </table>
</form>
*************************************************************************
9. In Views/Home


@{
    ViewData["Title"] = "Home Page";
}

<div class="text-center">
    <h1 class="display-4">Welcome</h1>
    
    <p>User Id : @ViewData["UserID"]</p>

    
        <div class="mb-3">
            <h3>Are you an employee or a farmer?</h3>
            <div>
                <input type="submit" onclick="location.href='@Url.Action("Index", "Farmer")' "value="Employee" />
                <input type="submit" onclick="location.href='@Url.Action("Index", "Product")' " value="Farmer" />

                
            </div>

        </div>
        
        </div>
*************************************************************************
10. In Views/Product

@model Farm_Central.Models.ProductViewModel

@{
    ViewData["Title"] = "Add Product";
    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr />
@if (!string.IsNullOrWhiteSpace(errorMessage))
{
<div class="alert alert-danger">
    <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
</div>

}
<form action="Create" method="post" autocomplete="off">
    <div class="mb-3">
        <label class="form-label">Farmers Full Name</label>
        <input type="text" class="form-control" asp-for="FarmerName" />
        <span asp-validation-for="FarmerName" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Name</label>
        <input type="text" class="form-control" asp-for="ProductName" />
        <span asp-validation-for="ProductName" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Description</label>>
        <input type="text" class="form-control" asp-for="ProductDescription" />
        <span asp-validation-for="ProductDescription" class="text-danger"></span>
    </div>


    <div class="mb-3">
        <label class="form-label">Sell-By-Date</label>
        <input type="date" class="form-control" asp-for="DateManufactured" />
        <span asp-validation-for="DateManufactured" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Price</label>
        <div class="input-group">
            <span class="input-group-text">R</span>
        <input type="text" class="form-control" asp-for="FarmerName" />
        <span asp-validation-for="FarmerName" class="text-danger"></span>
    </div>

    <div class="d-grid">
        <button type="submit" class="btn btn-primary">Submit</button>
    </div>
    </div>
</form>
@section Scripts{
    @{
        await Html.RenderPartialAsync("_ValidationScriptsPartial");
    }
}
                     *************************************
@model Farm_Central.Models.ProductViewModel


@{
    ViewData["Title"] = "Delete Product Details";

    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr />
@if (!string.IsNullOrWhiteSpace(errorMessage))
{
    <div class="alert alert-danger">
        <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>

}
<form action="Delete" method="post" autocomplete="off">
    <div class="mb-3">
        <label class="form-label">Id</label>
        <input type="text" class="form-control" asp-for="ProductId" readonly />
    </div>

    <div class="mb-3">
        <label class="form-label">Farmer's Name'</label>
        <input type="text" class="form-control" asp-for="FarmerName" readonly/>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Name</label>
        <input type="text" class="form-control" asp-for="ProductName" readonly/>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Description</label>
        <input type="text" class="form-control" asp-for="ProductDescription" readonly/>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Price</label>
        <div class="input-group">
            <span class="input-group-text">R</span>
            <input type="text" class="form-control" asp-for="ProductPrice" readonly/>
        </div>
     </div>
   

    <div class="mb-3">
        <label class="form-label">Sell-By-Date</label>
        <input type="date" class="form-control" asp-for="DateManufactured" readonly/>
      </div>

        <div class="d-grid">
            <button type="submit" class="btn btn-danger" onclick="return confirm('Are you sure you want to delete the product of farmer : @Model.FarmerName')? ">Delete</button>
        </div>
</form>


                    ****************************************
@model Farm_Central.Models.ProductViewModel
    

@{
    ViewData["Title"] = "Edit Product Details";

    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr/>
@if (!string.IsNullOrWhiteSpace(errorMessage))
{
    <div class="alert alert-danger">
        <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>

}
<form action="Edit" method="post" autocomplete="off">
    <div class="mb-3">
        <label class="form-label">Id</label>
        <input type="text" class="form-control" asp-for="ProductId" readonly />
    </div>

    <div class="mb-3">
        <label class="form-label">Farmer's Name'</label>
        <input type="text" class="form-control" asp-for="FarmerName" />
        <span asp-validation-for="FarmerName" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Name</label>
        <input type="text" class="form-control" asp-for="ProductName" />
        <span asp-validation-for="ProductName" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Description</label>
        <input type="text" class="form-control" asp-for="ProductDescription" />
        <span asp-validation-for="ProductDescription" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Product Price</label>
        <div class="input-group">
            <span class="input-group-text">R</span>
        <input type="text" class="form-control" asp-for="ProductPrice" />
        </div>
        <span asp-validation-for="ProductPrice" class="text-danger"></span>
    </div>

    <div class="mb-3">
        <label class="form-label">Sell-By-Date</label>
        <input type="date" class="form-control" asp-for="DateManufactured" />
        <span asp-validation-for="DateManufactured" class="text-danger"></span>
    </div>
    <div class="d-grid">
        <button type="submit" class="btn btn-success">Update</button>
    </div>

    
</form>
@section Scripts{
    @{
        await Html.RenderPartialAsync("_ValidationScriptsPartial");
    }
}
                      ***********************************
@model  List<Farm_Central.Models.ProductViewModel>;

@{
    ViewData["Title"] = "View Products";
    var successMessage = TempData["successMessage"]?.ToString();
    var errorMessage = TempData["errorMessage"]?.ToString();
}
<h3>@ViewData["Title"]</h3>
<hr />
@if (!string.IsNullOrWhiteSpace(successMessage))
{
    <div class="alert alert-success">
        <strong>Success! </strong>@successMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>
    
}
else if (!string.IsNullOrWhiteSpace(errorMessage))
{
    <div class="alert alert-danger">
        <strong>Error! </strong>@errorMessage
        <button type="button" class="btn-close float-end" data-bs-dismiss="alert"></button>
    </div>
    
}
<form>
    <button asp-action="Create" asp-controller="Product" class="btn btn-primary mb-3">Add A New Product</button>
    <table class="table table-responsive table-hover table-bordered">
        <thead>
            <tr class="table-active">
                <th>Product Id</th>
                <th>Farmers Full Name</th>
                <th>Product Name</th>
                <th>Product Description</th>
                <th>Product Price</th>
                <th> Manufactured or Sold By Date</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            @if (Model != null && Model.Any())
            {
                    @foreach (var product  in Model)
                    {
                    <tr>
                        <td>@product.ProductId</td>
                        <td>@product.FarmerName</td>
                        <td>@product.ProductName</td>
                        <td>@product.ProductDescription</td>
                        <td>@product.ProductPrice</td>
                        <td>@product.DateManufactured.ToString("dd/mm/yyyy") </td>
                        <td>
                            <div class="btn-group btn-group-sm">
                                <a asp-controller="Product" asp-action="Edit" asp-route-id="@product.ProductId" class="btn btn-primary">Edit</a>

                                <a asp-controller="Farmer" asp-action="Delete" asp-route-id="@product.ProductId" class="btn btn-danger">Delete</a>
                            </div>
                        </td>
                    </tr>
                }
            }
            else
            {
                <tr>
                    <td colspan="7">
                        <div>
                            No New Products Have Been Added To The Profiles!
                        </div>
                    </td>
                </tr>

            }
        </tbody>
    </table>
</form>
                                                      ***************************************
11. In the Views/Shared :

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - Farm_Central</title>
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
    <link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />
    <link rel="stylesheet" href="~/Farm_Central.styles.css" asp-append-version="true" />
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
</head>
<body>
    <header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
            <div class="container">
                <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">Farm_Central</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target=".navbar-collapse" aria-controls="navbarSupportedContent"
                        aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                   <partial name="_LoginPartial.cshtml"/>
                </div>
            </div>
        </nav>
    </header>
    <div class="container">
        <main role="main" class="pb-3">
            @RenderBody()
        </main>
    </div>

    <footer class="border-top footer text-muted">
        <div class="container">
            &copy; 2023 - Farm_Central - <a asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
        </div>
    </footer>
    <script src="~/js/customjs.js"></script>
    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
                        **********************************

@using Microsoft.AspNetCore.Identity
@using Farm_Central.Areas.Identity.Data

@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

<ul class="navbar-nav">
@if (SignInManager.IsSignedIn(User))
{
    <li class="nav-item">
        <a id="manage" class="nav-link text-dark" asp-area="Identity" asp-page="/Account/Manage/Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
    </li>
    <li class="nav-item">
        <form id="logoutForm" class="form-inline" asp-area="Identity" asp-page="/Account/Logout" asp-route-returnUrl="@Url.Action("Index", "Home", new { area = "" })">
            <button id="logout" type="submit" class="nav-link btn btn-link text-dark border-0">Logout</button>
        </form>
    </li>
}
else
{
    <li class="nav-item">
        <a class="nav-link text-dark" id="register" asp-area="Identity" asp-page="/Account/Register">Register</a>
    </li>
    <li class="nav-item">
        <a class="nav-link text-dark" id="login" asp-area="Identity" asp-page="/Account/Login">Login</a>
    </li>
}
</ul>
                   ************************************

@model ErrorViewModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed applications.</strong>
    It can result in displaying sensitive information from exceptions to end users.
    For local debugging, enable the <strong>Development</strong> environment by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>
    and restarting the app.
</p>
**************************************************************************
12. PS Make sure your connectionstring is correct
   appsettings.json :
     

{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "AuthDBContextConnection": "Server=(localdb)\\mssqllocaldb;Database=Farm_CentralDB;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=True;"
    } 
}
***************************************************************************
  13. In Program.cs :

 using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Farm_Central.Data;
using Farm_Central.Areas.Identity.Data;
using Farm_Central.DataAccessLayer;

var builder = WebApplication.CreateBuilder(args);
var connectionString = builder.Configuration.GetConnectionString("AuthDBContextConnection") ?? throw new InvalidOperationException("Connection string 'AuthDBContextConnection' not found.");

builder.Services.AddDbContext<AuthDBContext>(options => options.UseSqlServer(connectionString));

builder.Services.AddDefaultIdentity<ApplicationUser>(options => options.SignIn.RequireConfirmedAccount = false)
    .AddEntityFrameworkStores<AuthDBContext>();

// Add services to the container.
builder.Services.AddControllersWithViews();
builder.Services.AddRazorPages();

//DI
builder.Services.AddDbContext<FarmerDbContext>(options =>
options.UseSqlServer(builder.Configuration.GetConnectionString("AuthDbContextConnection"))
);
builder.Services.AddDbContext<ProductDbContext>(options =>
options.UseSqlServer(builder.Configuration.GetConnectionString("AuthDbContextConnection"))
);

builder.Services.Configure<IdentityOptions>(options =>
{
    options.Password.RequireUppercase = false;
});

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();

app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
app.MapRazorPages();

app.Run();

************************************************************************

To Run the Application:

1) To run the  application, you need to select click on 'Debug' and then 'Start Debugging' or press F5. 


  
    






