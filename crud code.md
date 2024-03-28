using Myfirstwebpage.manish;
using Myfirstwebpage.Models;
using System;
using System.Collections.Generic;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Web;
using System.Web.Mvc;

namespace Myfirstwebpage.Controllers
{
    public class HomeController : Controller
    {
        NUMMEntities dbcon = new NUMMEntities();
        public ActionResult Index()
        {
            List<mymodel> employees = new List<mymodel>();
          
            var data =dbcon.EMPDATAs.ToList();

            foreach (var item in data)
            {
                employees.Add(new mymodel
                {
                    ID = item.ID,
                    NAME=item.NAME,
                    MOBILE=item.MOBILE,
                    SALARY=item.SALARY,
                    POST=item.POST
                }); 
                
            }

            return View(employees);
        }

        public ActionResult About()
        {
            ViewBag.Message = "Your application description page.";

            return View();
        }

        public ActionResult Contact()
        {
            ViewBag.Message = "Your contact page.";

            return View();
        } public ActionResult AddEmp()
        {
            

            return View();
        }

        [HttpPost]
        public ActionResult AddEmp(mymodel std)
        {
            EMPDATA employee = new EMPDATA();
            employee.ID = std.ID;
            employee.NAME = std.NAME;
            employee.MOBILE = std.MOBILE;
            employee.SALARY = std.SALARY;
            employee.POST = std.POST;
           
           

            if (std.ID == 0)
            {
               dbcon.EMPDATAs.Add(employee);
                dbcon.SaveChanges();
            }
            else
            {
                dbcon.Entry(employee).State = System.Data.Entity.EntityState.Modified;
                dbcon.SaveChanges();
            }

            return RedirectToAction("index");
        }

        public ActionResult Edit(int id)
        {
            var editdata = dbcon.EMPDATAs.Where(a => a.ID == id).FirstOrDefault();
            EMPDATA employee = new EMPDATA();
            employee.ID = editdata.ID;
            employee.NAME = editdata.NAME;
            employee.MOBILE = editdata.MOBILE;
            employee.SALARY = editdata.SALARY;
            employee.POST = editdata.POST;

           
            return View("AddEmp", employee);
        }
        public ActionResult Delete(int id)
        {
            var deletedata = dbcon.EMPDATAs.Where(a => a.ID == id).FirstOrDefault();
            if (deletedata != null)
            {
                dbcon.EMPDATAs.Remove(deletedata);
                dbcon.SaveChanges();
            }
            return RedirectToAction("index");
        }

    }
}
   
    
==========================index===============

@model List<Myfirstwebpage.Models.mymodel>
@{
    ViewBag.Title = "Index";
}

<h2>Index</h2>
<a href="@Url.Action("AddEmp", "Home")" class="btn btn-primary">Add Employee</a>
<table class="table-bordered table-dark table table-responsive">
    <tr>
        <td>ID</td>
        <td>NAME</td>
        <td>MOBILE</td>
        <td>SALARY</td>
        <td>POST</td>
        <td>edit</td>
        <td>delete </td>
    </tr>
    @foreach(var item in Model)
            {
                <tr>
                    <td>@item.ID</td>
                    <td>@item.NAME</td>
                    <td>@item.MOBILE</td>
                    <td>@item.SALARY</td>
                    <td>@item.POST</td>
                    <td><a href="@Url.Action("Edit","Home", new {item.ID} )" class="btn btn-outline-warning">Edit</a></td>

                    <td><a href="@Url.Action("Delete","Home", new {item.ID})" class="btn btn-outline-danger">Delete</a></td>

                </tr>
            }
</table>

============addemp===============
@model Myfirstwebpage.Models.mymodel
@{
    ViewBag.Title = "AddEmp";
}

<h2>AddEmp</h2>


@using (Html.BeginForm("AddEmp", "Home", FormMethod.Post))
{
    @Html.HiddenFor(a => a.ID)

    <div>
        <label class="form-group">Name</label>
        @Html.TextBoxFor(m => m.NAME, new { @class = "form-control", @placeholder = "Name", @required = "required" })

    </div>
    <div>
        <label class="form-group">Mobile</label>
        @Html.TextBoxFor(m => m.MOBILE, new { @class = "form-control", @placeholder = "mobile", @required = "required" })
    </div>
    <div>
        <label class="form-group">Salary</label>
        @Html.TextBoxFor(m => m.SALARY, new { @class = "form-control", @placeholder = "salary", @required = "required" })
    </div>
      
 

      
    

    if (ViewBag.Edit == "Update")
    {
        <button class="btn btn-primary" type="submit">Update</button>
    }

    else
    {

        <button class="btn btn-primary" type="submit">Submit</button>
    }
    <button class="btn btn-success" type="reset">Reset</button>

}
