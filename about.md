---
title: About us
subtitle: About the company
img_path: images/stock/about.jpg
# menus:
#   secondary:
#     weight: 1
#     title: About Us
layout: page
---
This is the azimuth theme. You can find out more info about customizing your theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com)



```html
<head>
  <meta link=""/>
</head>
```

You can find the source code for the Jekyll new theme at: [github.com/jglovier/jekyll-new](https://github.com/jglovier/jekyll-new)

You can find the source code for Jekyll at [github.com/jekyll/jekyll](https://github.com/jekyll/jekyll)

Link to md file [Services]({% link services.md %})

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
	if (env.IsDevelopment())
	{
		app.UseDeveloperExceptionPage();
		app.UseDatabaseErrorPage();
	}
	else // Production
	{
		app.UseExceptionHandler("/Error");
		app.UseHsts(); // HTTPS Strict mode
	}

	app.UseHttpsRedirection(); // Redirects HTTP to HTTPS
	app.UseStaticFiles();
	app.UseCookiePolicy();

	app.UseAuthentication();

	app.UseMvc();
}
```