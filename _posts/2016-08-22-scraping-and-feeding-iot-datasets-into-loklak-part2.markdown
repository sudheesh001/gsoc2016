---
layout: post
title:  "Scraping and Feeding IOT Datasets into Loklak - Part 2"
date:   2016-08-22 00:18:23 
categories: loklak
---

As the integrations for the IOT services had begun, there are challenges especially with scraping multiple pages at once, such was the case with the NOAA Alerts and Weather information of the US Government. To scrape this information for the live updates that happen every 5 minutes it was necessary to simplify the process with which this process could be completed without running a complex web scraper work on them every time taking up precious cycles, What’s really interesting about the website is the way in which the data can be modeled into XML for any given page. Using this and leveraging the XML and other data conversion logic implemented previously for such task, I started digging deeper into the total working of the website and realized that appending &y=0 to the alerts URL resulted in XML generation, here’s an example of how this works
https://alerts.weather.gov/cap/wwaatmget.php?x=AKC013&y=0
and
https://alerts.weather.gov/cap/wwaatmget.php?x=AKC013

<img src="{{ site.baseurl }}/img/iot_1.png" alt="IOT NOAA Website" width="100%">

Equivalent XML being

<img src="{{ site.baseurl }}/img/iot_xml.png" alt="IOT NOAA Website XML" width="100%">

So extracting this has become quite a challenge because this poses two different challenges , one is how we can efficiently retrieve the information of the counties and how we can construct the alert urls. Perl to the rescue here !

{% highlight perl %}
sub process_statelist {
    my $html = `wget -O- -q https://alerts.weather.gov/`;
    $html =~ s@.*summary="Table summary@@s;
    $html =~ s@.*\s*@@s;
    $html =~ s@\s*.*@@s;
    $html =~ s@\s*@@s;
    %seen = ();

    while ( $html =~ m@cap/(\w+?)\.php\?x=1">([^<]+)@sg ) {
        my $code = $1;
        my $name = $2;
        $name =~ s/'/\\'/g;
        $name =~ s@\s+@ @g;
        if (!exists($seen{$code})) {
            push @states_entries, $name;
            push @states_entryValues, $code;
        }
        $seen{$code} = 1;
    }
    open STATE, ">", "states.xml";
    print STATE <<EOF1;




    
EOF1
    foreach my $entry (@states_entries) {
        my $temp = $entry;
        $temp =~ s/'/\\'/g;
        $temp = escapeHTML($temp);
        print STATE "        $temp\n";
    }
    print STATE <<EOF2;
    
    
EOF2
    foreach my $entryValue (@states_entryValues) {
        my $temp = $entryValue;
        print STATE "        $temp\n";
    }
    print STATE <<EOF3;
    


EOF3
    close STATE;
    print "Wrote states.xml.\n";
}
{% endhighlight %}

Makes a request to the website and constructs the states list of all the states present in the USA. Now it’s time to construct it’s counties.

{% highlight perl %}
sub process_state {
    my $state = shift @_;
    if ( $state !~ /^[a-z]+$/ ) {
        print "Invalid state code: $state (skipped)\n";
        return;
    }

    my $html = `wget -O- -q https://alerts.weather.gov/cap/${state}.php?x=3`;

    my @entries     = ();
    my @entryValues = ();

    $html =~ s@.*@@s;
    while ( $html =~
m@\s*?]+>\s*?]+>\s*?]+>\s*?\s*?]+>\s*?]+>([^<]+)\s*?\s*?]+>([^<]+)\s*?\s*@mg
      )
    {
        push @entries,     $2;
        push @entryValues, $1;
    }
    my $unittype = "Entire State";
    if ($state =~ /^mz/) {
        $unittype = "Entire Marine Zone";
    }
    if ($state eq "dc") {
        $unittype = "Entire District";
    }
    if (grep { $_ eq $state } qw(as gu mp um vi) ) {
        $unittype = "Entire Territory";
    }
    if ($state eq "us") {
        $unittype = "Entire Country";
    }
    if ($state eq "mzus") {
        $unittype = "All Marine Zones";
    }
    print COUNTIES <<EOF1;
    
        $unittype
EOF1
    foreach my $entry (@entries) {
        my $temp = $entry;
        $temp =~ s/'/\\'/g;
        $temp = escapeHTML($temp);
        print COUNTIES "        $temp\n";
    }
    print COUNTIES <<EOF2;
    
    
        https://alerts.weather.gov/cap/$state.php?x=0
EOF2
    foreach my $entryValue (@entryValues) {
        my $temp = $entryValue;
        $temp =~ s/'/\\'/g;
        $temp = escapeHTML($temp);
        print COUNTIES "        https://alerts.weather.gov/cap/wwaatmget.php?x=$temp&y=0\n";
    }
    print COUNTIES <<EOF3;
    
EOF3
    print "Processed counties from $state.\n";

}
{% endhighlight %}

There we go voila, we now have a perfect mapping in between every single county and the alert URL requirement for that particular county. The NOAA scraper and parser has been quite a challenge but provides the data in real-time from the loklak server. The information can be passed via the XML Parser written as a service at `/api/xml2json.json` and the developers can receive the information in their required format.
