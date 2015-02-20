Mojolicious_Startup
===================
```perl

package Fallsstudio;
use Mojo::Base 'Mojolicious';

use DBI;

has dbh => sub {
    my $self = shift;
    return DBI->connect( "dbi:mysql:database=db", 
        'root', 'password', {RaiseError => 1} );
};

# This method will run once at server start
sub startup {
    my $self = shift;

    $self->log->level('debug');
    $self->plugin(
        mail => {
            from => 'web@domain.com',
            reply_to => 'jimi@gmail.com',
            encoding => 'base64',
            how => 'sendmail',
            howargs => [ '/usr/sbin/sendmail -t' ],
    });

    # Router
    my $r = $self->routes;

    # Normal route to controller
    $r->any('/')->to('falls#index');
    $r->any('/send_msg')->to('falls#email');
    $r->any('/exp_json')->to('falls#return_json');
    $r->any('/stash_json')->to('falls#return_stash_json');
}

1;

```
