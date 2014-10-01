TEST
====
use LWP::Simple;
use LWP::UserAgent;
use HTTP::Request;
use HTTP::Response;
use HTML::LinkExtor;

  
my $url_count = 0;
my $url_number = 0;
my @urls = ('https://tw.yahoo.com/');
my %visited;  
my %content;
my $browser = LWP::UserAgent->new();
$browser->timeout(5);

while (@urls) {
  my $url = shift @urls;

  # Skip this URL and go on to the next one if we've
  # seen it before
  if ( $visited{$url} ) { ##check if URL already exists
    next;
  }
  else { 
    $url_count++ ;
	$visited{$url} = 0 ;
  }	
  my $request = HTTP::Request->new(GET => $url);
  my $response = $browser->request($request);

  # No real need to invoke printf if we're not doing
  # any formatting
  if ($response->is_error()) { 
    #print $response->status_line, "\n"; 
  } #if
  my $contents = $response->content();

  # Now that we've got the url's content, mark it as
  # visited
  $visited{$url} = 1;

  my ($page_parser) = HTML::LinkExtor->new(undef, $url);
  $page_parser->parse($contents)->eof;
  my @links = $page_parser->links;

  foreach my $link (@links) {
    if ( $$link[2] =~ /\.jpg/ ) {
	  
	} #
	   
	elsif ( $$link[2] =~ /\.gif/ ) {
	
	} #

	elsif ( $$link[2] =~ /\.png/ ) {
	
	} #
	elsif ( $$link[2] =~ /\.ico/ ) {
	
	} #
	elsif ( $$link[2] =~ /\.css/ ) {
	
	} #	
	elsif ( $$link[2] =~ /mailto\:/ ) {
	
	} #	
	
	else {
      if( !exists $visited{$$link[2]} ) {
	    $visited{$$link[2]} = 0 ;
		#$content{$$link[2]} = $contents ;
		
		open my $file1,">>", ("$url_number".".html");
        my $request = HTTP::Request->new(GET => $$link[2]);
        my $response = $browser->request($request);

        if ($response->is_error()) { 
        } #if
		
        my $contents = $response->content();
		print $file1 $contents; 
		close $file1;
		$url_number = $url_number + 1 ;
	  } #if

	  push @urls, $$link[2];
	}  

  }
  
  if ( $url_count == 10) {
    last;
  }
  
  
}

