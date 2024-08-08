- MVC: Model View Controller (separation of concerns)
- MVC is a design pattern used to develop interactive application
- Not only for web but can be used for desktop and mobile as well
- domain model (Model) and business logic (Controller) is separated from UI (View)


![[Pasted image 20240808120446.png]]

Model represents the application state and business logic, most of the tim it will be the data Model(schema) and Service (business layer)


example of controller in MVC.
```c#
using ASPNETCoreMVCApplication.Models;
using Microsoft.AspNetCore.Mvc;

namespace ASPNETCoreMVCApplication.Controllers
{
    public class StudentController : Controller
    {
        public ActionResult Details(int studentId)
        {
            StudentBusinessLayer studentBL = new StudentBusinessLayer();
            Student studentDetail = studentBL.GetById(studentId);

            return View(studentDetail);
        }
    }
}
```


