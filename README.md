[![CPAN version](https://badge.fury.io/pl/Business-GL-Postalcode.svg)](http://badge.fury.io/pl/Business-GL-Postalcode)
[![Build Status](https://travis-ci.org/jonasbn/perl-Business-GL-Postalcode.svg?branch=master)](https://travis-ci.org/jonasbn/perl-Business-GL-Postalcode)
[![Coverage Status](https://coveralls.io/repos/jonasbn/perl-Business-GL-Postalcode/badge.png)](https://coveralls.io/r/jonasbn/perl-Business-GL-Postalcode)

# NAME

Business::GL::Postalcode - Greenland postal code validator and container

# VERSION

This documentation describes version 0.02

# SYNOPSIS

    # basic validation of string
    use Business::GL::Postalcode qw(validate);

    if (validate($postalcode)) {
        print "We have a valid Greenland postal code\n";
    } else {
        warn "Not a valid Greenland postal code\n";
    }


    # basic validation of string, using less intrusive subroutine
    use Business::GL::Postalcode qw(validate_postalcode);

    if (validate_postalcode($postalcode)) {
        print "We have a valid Greenland postal code\n";
    } else {
        warn "Not a valid Greenland postal code\n";
    }


    # using the untainted return value
    use Business::GL::Postalcode qw(validate_postalcode);

    if (my $untainted = validate_postalcode($postalcode)) {
        print "We have a valid Greenland postal code: $untainted\n";
    } else {
        warn "Not a valid Greenland postal code\n";
    }


    # All postal codes for use outside this module
    use Business::GL::Postalcode qw(get_all_postalcodes);

    my @postalcodes = @{get_all_postalcodes()};


    # All postal codes and data for use outside this module
    use Business::GL::Postalcode qw(get_all_data);

    my $postalcodes = get_all_data();

    foreach (@{postalcodes}) {
        printf
            'postal code: %s city: %s street/desc: %s company: %s province: %d country: %d', split /;/, $_, 6;
    }

# FEATURES

- Providing list of Greenland postal codes and related area names
- Look up methods for Greenland postal codes for web applications and the like
- The postal code for Santa Claus (father Christmas)

# DESCRIPTION

This distribution is not the original resource for the included data, but simply
acts as a simple distribution for Perl use. The central source is monitored so this
distribution can contain the newest data. The monitor script (`postdanmark.pl`) is
included in the distribution: [https://metacpan.org/pod/Business::DK::Postalcode](https://metacpan.org/pod/Business::DK::Postalcode).

The data are converted for inclusion in this module. You can use different extraction
subroutines depending on your needs:

- ["get\_all\_data"](#get_all_data), to retrieve all data, data description below in ["Data"](#data).
- ["get\_all\_postalcodes"](#get_all_postalcodes), to retrieve all postal codes
- ["get\_all\_cities"](#get_all_cities), to retieve all cities
- ["get\_postalcode\_from\_city"](#get_postalcode_from_city), to retrieve one or more postal codes from a city name
- ["get\_city\_from\_postalcode"](#get_city_from_postalcode), to retieve a city name from a postal code

## Data

Here follows a description of the included data, based on the description from
the original source and the authors interpretation of the data, including
details on the distribution of the data.

### city name

A non-unique, case-sensitive representation of a city name in Greenlandic or Danish.

### street/description

This field is unused for this dataset.

### company name

This field is unused for this dataset.

### province

This field is a bit special and it's use is expected to be related to distribution
all entries are marked as 'False'. The data are included since they are a part of
the original data.

### country

Since the original source contains data on 3 different countries:

- Denmark (1)
- Greenland (2)
- Faroe Islands (3)

Only the data representing Greenland has been included in this distribution, so this
field is always containing a '2'.

For access to the data on Denmark or Faroe Islands please refer to: [Business::DK::Postalcode](https://metacpan.org/pod/Business::DK::Postalcode)
and [Business::FO::Postalcode](https://metacpan.org/pod/Business::FO::Postalcode) respectfully.

## Encoding

The data distributed are in Greenlandic and Danish for descriptions and names and these are encoded in UTF-8.

# SUBROUTINES AND METHODS

## validate

A simple validator for Greenlandic postal codes.

Takes a string representing a possible Greenlandic postal code and returns either
**1** or **0** indicating either validity or invalidity.

    my $rv = validate(3900);

    if ($rv == 1) {
        print "We have a valid Greenlandic postal code\n";
    } ($rv == 0) {
        print "Not a valid Greenlandic postal code\n";
    }

## validate\_postalcode

A less intrusive subroutine for import. Acts as a wrapper of ["validate"](#validate).

    my $rv = validate_postalcode(2412);

    if ($rv) {
        print "We have a valid Greenlandic postal code\n";
    } else {
        print "Not a valid Greenlandic postal code\n";
    }

## get\_all\_data

Returns a reference to a a list of strings, separated by tab characters. See
["Data"](#data) for a description of the fields.

    use Business::GL::Postalcode qw(get_all_data);

    my $postalcodes = get_all_data();

    foreach (@{postalcodes}) {
        printf
            'postalcode: %s city: %s street/desc: %s company: %s province: %d country: %d', split /\t/, $_, 6;
    }

## get\_all\_postalcodes

Takes no parameters.

Returns a reference to an array containing all valid Danish postal codes.

    use Business::GL::Postalcode qw(get_all_postalcodes);

    my $postalcodes = get_all_postalcodes;

    foreach my $postalcode (@{$postalcodes}) { ... }

## get\_all\_cities

Takes no parameters.

Returns a reference to an array containing all Danish city names having a postal code.

    use Business::GL::Postalcode qw(get_all_cities);

    my $cities = get_all_cities;

    foreach my $city (@{$cities}) { ... }

Please note that this data source used in this distribution by no means is authorative
when it comes to cities located in Denmark, it might have all cities listed, but
unfortunately also other post distribution data.

## get\_city\_from\_postalcode

Takes a string representing a Danish postal code.

Returns a single string representing the related city name or an empty string indicating nothing was found.

    use Business::GL::Postalcode qw(get_city_from_postalcode);

    my $zipcode = '3900';

    my $city = get_city_from_postalcode($zipcode);

    if ($city) {
        print "We found a city for $zipcode\n";
    } else {
        warn "No city found for $zipcode";
    }

## get\_postalcode\_from\_city

Takes a string representing a Danish/Greenlandic city name.

Returns a reference to an array containing zero or more postal codes related to that city name. Zero indicates nothing was found.

Please note that city names are not unique, hence the possibility of a list of postal codes.

    use Business::GL::Postalcode qw(get_postalcode_from_city);

    my $city = 'Nuuk';

    my $postalcodes = get_postalcode_from_city($city);

    if (scalar @{$postalcodes} == 1) {
        print "$city is unique\n";
    } elsif (scalar @{$postalcodes} > 1) {
        warn "$city is NOT unique\n";
    } else {
        die "$city not found\n";
    }

# DIAGNOSTICS

There are not special diagnostics apart from the ones related to the different
subroutines.

# CONFIGURATION AND ENVIRONMENT

This distribution requires no special configuration or environment.

# DEPENDENCIES

- [https://metacpan.org/pod/Carp](https://metacpan.org/pod/Carp) (core)
- [https://metacpan.org/pod/Exporter](https://metacpan.org/pod/Exporter) (core)
- [https://metacpan.org/pod/Data::Handle](https://metacpan.org/pod/Data::Handle)
- [https://metacpan.org/pod/List::Util](https://metacpan.org/pod/List::Util)
- [https://metacpan.org/pod/Params::Validate](https://metacpan.org/pod/Params::Validate)

## TEST

Please note that the above list does not reflect requirements for:

- Additional components in this distribution, see `lib/`. Additional
components list own requirements
- Test and build system, please see: `Build.PL` for details

# BUGS AND LIMITATIONS

There are no known bugs at this time.

The data source used in this distribution by no means is authorative when it
comes to cities located in Denmark, it might have all cities listed, but
unfortunately also other post distribution data.

# BUG REPORTING

Please report issues via CPAN RT:

- Web (RT): [http://rt.cpan.org/NoAuth/Bugs.html?Dist=Business-GL-Postalcode](http://rt.cpan.org/NoAuth/Bugs.html?Dist=Business-GL-Postalcode)
- Web (Github): [https://github.com/jonasbn/perl-Business-GL-Postalcode/issues](https://github.com/jonasbn/perl-Business-GL-Postalcode/issues)
- Email (RT): [bug-Business-GL-Postalcode@rt.cpan.org](https://metacpan.org/pod/bug-Business-GL-Postalcode@rt.cpan.org)

# SEE ALSO

- [Business::DK::Postalcode](https://metacpan.org/pod/Business::DK::Postalcode)
- [Business::FO::Postalcode](https://metacpan.org/pod/Business::FO::Postalcode)

# MOTIVATION

Postdanmark the largest danish postal and formerly stateowned postal service, maintain the
postalcode mapping for Greenland and the Faroe Islands. Since I am using this resource to
maintain the danish postalcodes I decided to release distributions of these two other countries.

# AUTHOR

Jonas B. Nielsen, (jonasbn) - `<jonasbn@cpan.org>`

# COPYRIGHT

Business-GL-Postalcode is (C) by Jonas B., (jonasbn) 2014-2019

Business-GL-Postalcode is released under the artistic license 2.0
