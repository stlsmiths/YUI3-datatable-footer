## DataTableFooter Plugin for YUI 3

Includes support for DataTableScroll and use of "fixed" (i.e. non-scrolling") footer.

Currently available "macros" are {SUM}, {AVG}, {MIN}, {MAX}, {ROW\_COUNT}, {COL\_COUNT}, {DATE}, {TIME}.

In the near future we'll be providing a custom function capability (via fnValue) .

A working example is available at [dt_footer_plugin_use.html](http://www.blunderalong.com/pub/yui3/dt_footer_plugin_use.html)

<http://www.blunderalong.com/pub/yui3/dtplugin_basic.jpg>

### How Does it Work?

**Constructing the TFOOT**

For the simplest use case, with a basic DataTable to add a footer you simply add a TFOOT element to the TABLE DOM element.
The TFOOT would contain at least one row (via TR) and cells (via TH or TD).   

If the DataTable includes the Scrolling feature (via DataTableScroll plugin) things may get more _complicated_.  In most 
cases when scrolling is involved, the user will want the footer to be "fixed" at the bottom row of the table.  Accomplishing 
this "fixed" footer requires (within YUI3) the following;
* Appending a DIV to the DataTable container Node
* Adding a TABLE and TFOOT to the footer DIV from above
* _most importantly_, the footer TABLE is now an independent table, that requires "sync" of column widths with the "real" DT