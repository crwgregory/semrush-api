# SEMrush API client
### This repository has been forked from the [Silktide/semrush-api](https://github.com/silktide/semrush-api) repository and been made available on [Packagist](https://packagist.org/packages/nativerank/silktide-semrush-api) so to allow [Pagekit to install it as a dependency.](https://github.com/pagekit/pagekit/issues/630)
#### There has been a single change to the code base in the ResponseParser that's diverged this fork from the main API.
#### As such, the name of the project in the composer.json has been changed from 'silktide/semrush-api' to 'nativerank/silktide-semrush-api' to define the creator of the API, but make it specific to Nativeranks needs.

[![Build Status](https://travis-ci.org/silktide/semrush-api.svg?branch=master)](https://travis-ci.org/silktide/semrush-api)
[![Code Climate](https://codeclimate.com/github/silktide/semrush-api/badges/gpa.svg)](https://codeclimate.com/github/silktide/semrush-api)
[![Test Coverage](https://codeclimate.com/github/silktide/semrush-api/badges/coverage.svg)](https://codeclimate.com/github/silktide/semrush-api)

A PHP API client for the SEMrush API.

## Supported actions:

* domain_ranks
* domain_rank
* domain_rank_history
* domain_organic
* domain_adwords
* domain_adwords_unique

## Usage

### Installation

    composer require silktide/semrush-api
    composer update

### Setup

This library was designed to use Dependency Injection (DI). If you don't use DI, you could use the factory to set up the API client:

    $client = \Silktide\SemRushApi\ClientFactory::create("[YOUR SEMRUSH API KEY]");
    
### Caching

The API library can use a cache to reduce calls to the API.  This library comes with an in-memory cache:
    
    $cache = new \Silktide\SemRushApi\Cache\MemoryCache();
    $client->setCache($cache);
    
You can also write your own cache using the provided CacheInterface (`\Silktide\SemRushApi\Cache\CacheInterface`).
    
## API calls
        
### Domain ranks

Getting the SEMrush "domain_ranks" for a website:

    $result = $client->getDomainRanks('silktide.com');
        
### Domain rank

Getting the SEMrush "domain_rank" for a website:

        $result = $client->getDomainRank(
            'silktide.com', 
            [
                'database' => \Silktide\SemRushApi\Data\Database::DATABASE_GOOGLE_US
            ]
        );
        
### Domain rank history

Getting the SEMrush "domain_rank_history" for a website:

        $result = $client->getDomainRankHistory(
            'silktide.com', 
            [
                'database' => \Silktide\SemRushApi\Data\Database::DATABASE_GOOGLE_US
            ]
        );
        
### Domain organic

Getting the SEMrush "domain_organic" for a website:

        $result = $client->getDomainOrganic(
            'silktide.com', 
            [
                'database' => \Silktide\SemRushApi\Data\Database::DATABASE_GOOGLE_US
            ]
        );
        
### Domain adwords

Getting the SEMrush "domain_adwords" for a website:

        $result = $client->getDomainAdwords(
            'silktide.com', 
            [
                'database' => \Silktide\SemRushApi\Data\Database::DATABASE_GOOGLE_US
            ]
        );
        
### Domain adwords unique

Getting the SEMrush "domain_adwords_unique" for a website:

        $result = $client->getDomainAdwordsUnique(
            'silktide.com', 
            [
                'database' => \Silktide\SemRushApi\Data\Database::DATABASE_GOOGLE_US
            ]
        );
        
## Using options

Here's an example of passing options to the domain ranks action in order to return a specific set of columns.

    $result = $client->getDomainRanks('silktide.com', [
        'export_columns' => [
            \Silktide\SemRushApi\Data\Column::COLUMN_OVERVIEW_ADWORDS_BUDGET,
            \Silktide\SemRushApi\Data\Column::COLUMN_OVERVIEW_ADWORDS_KEYWORDS,
            \Silktide\SemRushApi\Data\Column::COLUMN_OVERVIEW_ADWORDS_TRAFFIC
         ]
    ]);
    
## Using results

All API actions will return a `Result` object.  Result objects contain a number of `Row` objects and are iterable and
countable.  Here's a (non exhaustive) example of how they can be used. 

    foreach ($result as $row) {
        $budget = $row->getValue(\Silktide\SemRushApi\Data\Column::COLUMN_OVERVIEW_ADWORDS_BUDGET);
        echo "\nThe AdWords spend of this site in the last month was an estimated ${$budget}";
    }