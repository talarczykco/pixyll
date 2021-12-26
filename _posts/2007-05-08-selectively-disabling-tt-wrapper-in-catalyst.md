---
id: 77
categories:
- Tech
title: Selectively disabling TT wrapper in Catalyst
layout: post
wpurl: https://www.mijit.com/?p=77
slug: 2007-05-08-selectively-disabling-tt-wrapper-in-catalyst
---
The following is a method for <a href="https://www.catalystframework.org/">Catalyst</a> sites using <a href="https://www.template-toolkit.org/">Template Toolkit</a> with a site-wide WRAPPER directive to selectively return partial HTML for the purpose of serving Asynchronous HTML and HTTP (ie, "AHAH').

Catalyst is a powerful and versatile web development framework that aims to be Perl's counterpart to Ruby on Rails, fusing a Model-View-Controller (MVC) environment with the depth, breadth, and power of CPAN. If you build web sites with <a href="https://www.perl.org/">Perl</a>, you should check it out.

The problem: you want to use TT's WRAPPER directive to supply a site-wide wrapper template, but you also want to be able to selectively turn the wrapper off so that you can serve some pages as partial HTML. At first glance, Catalyst's config() method might suggest that one could reconfigure your view in your action's end(), but that proves fruitless and it clucks in the following manner:

<code>[warn] Setting config after setup has been run is not a good idea. </code>

My solution: create an <code>end()</code> in your controller that emits "partial" HTML fragments by creating a new Template Toolkit object with a locally-defined configuration. (Note that since we override the root controller's <code>end()</code>, we must call forward/detach to it if we do not specify the "partial" parameter.)

<code>sub end : Private {
    my ($self, $c, $file) = @_;
    my $params = $c->request->parameters;
    if (exists $params->{'partial'}) {
        my $html;
        my $tt = Template->new({
            %{ $c->config->{'View::TT'} },
            INCLUDE_PATH => $c->path_to('root'),
            WRAPPER => undef,
        });
        $tt->process($file, $c->stash, \$html) or $c->log->warn($tt->error);
        $c->response->content_type('text/html');
        $c->response->body($html);
    }
    else {
        $c->detach(qw/CE::Controller::Root end/);
    }
}
</code>

A more robust solution better-suited for larger projects might be a new view with an alternate configuration (eg, a custom View::TTpartial,) but I wanted to show the logic directly in my controller, allowing full control of the configuration and direct access to the $html.

Coming soon: the "data island" solution I ultimately opted for to obviate AHAH for serving map marker data to a Google map.
