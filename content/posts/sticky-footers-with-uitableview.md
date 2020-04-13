---
title: 'Sticky Footers with UITableView'
date: Mon, 16 Feb 2015 23:49:00 +0000
draft: false
tags: ['Cocoa Touch']
---

The Problem
===========

Last week, I had to implement sticky footers in a table view. Sticky footers are where the footer comes at the end of all the table contents, but if the contents are such that the scrollable content height of the table view is less than the height of the view, the footer will still appear at the bottom of the view. 

{{<figure src="/images/nonstickyfooter-169x300.png" alt="A non-sticky footer" caption="A non-sticky footer">}}

{{<figure src="/images/stickyfooter-169x300.png" alt="A sticky footer" caption="A sticky footer">}}

I searched for a solution to this, but all I found were solutions where the footer was always at the bottom of the view. That's pretty easy to solve. You just use a UIViewController and put a UITableView and your footer view into the view controller's view, lay it out properly and you're good to go. In this scenario, the footer never scrolls and is always visible. That's not the kind of sticky footer I needed.

The Solution
============

The solution I came up with was to stick my footer view into another view that I used as the UITableView's footer. Let's call it the main footer view. You then resize this main footer view depending on the scrollable content of the table. If the scrollable content height is greater than the tables view's visible height then no need to do anything. The main footer view and my footer view are the same size. However, if the content height is less than the visible height then you just resize the main footer view to occupy the remaining space.

The Code
========

First, I added a propery to my UITableViewController subclass: `@property (nonatomic) BOOL footerNeedsLayout;` Then I overrode viewDidLayoutSubviews:

```objective-c
- (void)viewDidLayoutSubviews {
    [super viewDidLayoutSubviews];
    if (self.footerNeedsLayout) {
        [self setupComplianceFooter];
    }
} 
```

What this does is recreate the footer and position it after everything has been laid out. What this means is that whenever your data changes and before you call `reloadData` on the table you should set `footerNeedsLayout` to `YES`. This could have been done with programmatic constraints and let AutoLayout take care of it, but I chose to do it this way as it seemed to be a simpler solution and easier to get done in the time constraints I was under. Plus I wanted the compliance footer to be a reusable component and didn't want to duplicate the UI all over the storyboard for the app. 

Adding the footer works like this:

```objective-c
- (void)setupComplianceFooter {
    self.tableView.tableFooterView = nil;
    self.complianceViewController = [[ComplianceViewController alloc] init];
    self.complianceViewController.showAllComponentsIfData = YES;
    self.complianceViewController.view.autoresizingMask = UIViewAutoresizingFlexibleTopMargin | UIViewAutoresizingFlexibleWidth;

    UIView *complianceView = self.complianceViewController.view;
    UIView *footerView = [[UIView alloc] initWithFrame:complianceView.frame];
    footerView.autoresizingMask = UIViewAutoresizingFlexibleWidth;
    [footerView addSubview:complianceView];

    footerView.frame = [self frameForFooter:footerView];
    self.tableView.tableFooterView = footerView;

    [self addChildViewController:self.complianceViewController];
    self.footerNeedsLayout = NO;
} 
```

Notice that we do set the autoresizing masks of the compliance footer and the main footer so that AutoLayout will generate the correct constraints. 

Finally, here's the code to compute the frame for our main footer view:

```objective-c
- (CGRect)frameForFooter:(UIView *)footerView {
    CGRect frame = footerView.frame;
    CGFloat visibleSpace = CGRectGetHeight(self.tableView.frame) self.tableView.contentInset.bottom - self.tableView.contentInset.top;
    if (self.tableView.contentSize.height < visibleSpace - CGRectGetHeight(frame)) {
        CGFloat emptySpace = visibleSpace - self.tableView.contentSize.height;
        frame.size.height = emptySpace;
    }
    else {
        frame = self.complianceViewController.view.frame;
    }

    return frame;
} 
```

We just compute the visible space of the table view (its height - its content insets). If the height of the table view's content is less than the visible space - the footer height then we adjust the footer to fill up the empty space. Otherwise we leave the footer alone. It wasn't as hard as a thought it would be. A better solution would be to use layout constraints in such a way that the footer wouldn't have to be constantly recreated and repositioned whenever the data changes.
