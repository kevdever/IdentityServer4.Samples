# Asp.Net Core 2.1 Quickstart

### Combined_AspNetIdentity_and_EntityFrameworkStorage

This fork is focused on the 'Combined_AspNetIdentity_and_EntityFrameworkStorage' quickstart and porting it to Asp.Net Core 2.1.

To that end, that solution was updated by adding a new Server project alongside the Asp.Net Core 2.0 version.

While Asp.Net Core Identity is used to serve as the user store, it's UI is not used. The reasons include:
..*Identity adds an Area, and Identity UI middleware references controllers and views in that area, thus all routes account routes are now prepended with "/identity".  This default cannot be changed, but there is a workaround:
In the server when you add MVC middleware, add a new route rule to essentially remove "/identity" from the route.
e.g.,

```
  services.AddMvc()
        .AddRazorPagesOptions(o => o.Conventions.AddAreaFolderRouteModelConvention("Identity", "/Account/", model => 
        {
            foreach (var selector in model.Selectors)
            {
                var attributeRouteModel = selector.AttributeRouteModel;
                attributeRouteModel.Order = -1;
                attributeRouteModel.Template = attributeRouteModel.Template.Remove(0, "Identity".Length);
            }
        }))
        .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```
                
* The UI, by default (it you use AddDefaultUI when adding AddIdentity), is completely imported; there are no controllers or views to modify directly. You have the option to scaffold all of the views, if desired.  However, you do not get the controllers; they remain as imports.
As a consequence, controller modifications are not as straightforward as with Identity 2.0 and earlier.

* The default controller methods limit and/or break some expected functionality, especially regarding logout.  The IdentityUi controller only has a POST Logout method, while the Quickstarts assume two: a GET and a POST. The end result is that users, upon logout, are not given a redirect link.

* Because of the issues and challenges presented by IdentityUi, the QuickstartUi with IdentityServer4 is preferred in this project.


Items of note:
1. Migrations/db initializations need to be manually applied. See `DatabaseInit_README.txt`.
2. Seeding requires that the server be started with a cli flag of "/seed".  This has been automated by adding it directly into the project debug settings.
3. The Identity Area added by Asp.Net Core Identity was removed.
4. A client was added supporting the JS client.
5. In the API project, CORS middleware was added to support calls from the JS client.
6. The "legacy" IdentityServerWithAspIdAndEF was left in the solution.
