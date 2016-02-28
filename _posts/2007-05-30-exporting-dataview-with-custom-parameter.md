---
layout: post
status: publish
published: true
title: Exporting DataView with Custom Parameter
author: Bryan Napier
author_login: bryan
author_email: bryan.napier.md@gmail.com
wordpress_id: 11
wordpress_url: http://blog.ofonesandzeros.com/2007/05/30/exporting-dataview-with-custom-parameter/
date: '2007-05-30 11:31:02 -0400'
date_gmt: '2007-05-30 15:31:02 -0400'
categories:
- SharePoint
tags: []
comments: []
---
This past week I have been working with a DataView that needed to filter a list by the title of the PublishingPage containing the DataView web part. In order to provide this functionality, I created a straight-forward custom parameter:

{% highlight c# %}
public class PageTitleParameter : System.Web.UI.WebControls.Parameter
{
  protected override object Evaluate(System.Web.HttpContext context, System.Web.UI.Control control)   
  {
    SPListItem listItem = SPContext.Current.ListItem;

    if (listItem != null)
    {
      try
      {
        return listItem.Title;
      }
      catch
      {
        return string.Empty;
      }
    }

    return string.Empty;
  }
}
{% endhighlight %}

Creating custom Parameters has been [covered elsewhere](http://weblogs.asp.net/scottgu/archive/2006/01/23/436276.aspx), so I won’t bother going into that, it works pretty much the same in SharePoint as in any ASP.NET application.  Where things get different, is after you export your shiny-new DataView web part to SharePoint.  If your experience is anything like mine, when you add your web part to a page instead of seeing your rendered DataView, you see “Parsing Error:  Object reference not set to an instance of an object”.

So what went wrong?  If you look carefully at the parser error, you will likely see that the TagPrefix that you setup when you registered your custom parameter in SharePoint Designer has been replaced with something like “cc3”, “cc4”, etc.  In order to fix this, instead of exporting the web part from SPD straight into SharePoint, save it to a file first.  Open the .webpart file that you saved and add your @Register directive for “cc3” or “cc4” or whatever your custom parameter’s TagPrefix happened to become.  You can now go into the Web Part Gallery and upload your web part.

Good luck!
