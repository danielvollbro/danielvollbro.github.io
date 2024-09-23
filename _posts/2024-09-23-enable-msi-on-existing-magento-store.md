---
layout: post
title: Enable MSI on existing Magento store
description: How to enable MSI (Multi-Store Inventory) on an existing Magento store and how to fix the common errors.
date: 2024-09-22 21:12 +0200
image:
  path: /assets/img/posts/2024-09-23-enable-msi-on-existing-magento-store/chuttersnap-Q4bmoSPJM18-unsplash.webp
  alt: Topview of a shore with many containers
  lqip: /assets/img/posts/2024-09-23-enable-msi-on-existing-magento-store/chuttersnap-Q4bmoSPJM18-unsplash_lqip.webp
categories: [Magento, Guides]
tags: [magento, ecommerce]
---
I recently helped a friend of mine to upgrade their existing Magento store to the latest version (2.4.7-p2), with this I also upgraded all third-party modules that they had installed and one of the modules that imported products into Magento was rebuilt since previous version, around the MSI (Multi-Store Inventory) functionality in Magento so we had to enable those modules that was previously disabled and fix potential problems that occurred.

## What is MSI?

MSI is a feature in Magento that allows the users to setup multiple stores to manage inventory from that can be in different locations or with different sources, making it possible to sell products from different warehouses or physical stores. This feature was introduced back in version 2.3.0.

Even though my friends company only has one store and one source the third party module required me to enable this feature.

When enabling this feature and only has one source, there is a setting under Stores → Configuration that you can specify that you only runs a single store to reduce the amount of complexity it comes with setting up Magento for multiple stores.

## Enabling Modules

This was the easy part, all it took was to find all modules that needed to be enabled and put it the command below and running it in the terminal from Magento root folder.

```bash
php bin/magento module:enable <module 1> <module 2>...
```

Once the command was finished, Magento told me to re-compile the App code but before I did that I knew that one best-practis when adding or enabling modules is to run the store upgrade command to get database schemas and module configuration set so I did this.

```bash
php bin/magento setup:upgrade
```

Now that I was certain that everything was created and configured, I re-compile the App code.

```bash
php bin/magento setup:di:compile
```

With each module enabled and configured, database schemas created or updated and the App code compiled I was ready to test if everything still was working.

## Running the first test

I went to the storefront and immediately noticed problems, no products was shown. Okay I was not expecting everything to work at the first try, so I looked into the exception log.

```bash
less var/log/exceptions.log
```

I noticed that I got an error message telling `There are no source items with the in stock status` that most be some MSI functionality that has been added, lets add the missing source.

## Adding the missing source item

Went into the admin page that still worked after the update and then into Stores →Inventory → Sources and added a new source that I called ‘main source’.

While looked for where to add the source I also noticed the Stocks menu selection beneath the Sources menu selection, so went there and also added a new stock.

Awesome, that was easy, went back to to the storefront but still the same problem. What now? Then I remember, the first thing to always try when something is not working as expected in Magento, clear the cache.

```bash
# Clear cache
php bin/magento c:c
# Flush cache
php bin/magento c:f
```

Now with an fresh cache I went back to the storefront again, and voila, all products was back. There is nothing as satisfying as progress.

## Running a second test

Let’s open up a product and try to add it to the basket. First thing I noticed was that it said 0 in quantity on every product even the ones I knew that the had quantity in stock, so I added a note to look into.

Fortunatly the store allows backordering so even if there was no quantity in stock I should be able to place an order.

Tried to add the product to the basket but no that did not work, I was presented with the error message in the storefront, “No stock item found.”.

## Debugging the “No stock item found” error message

I thought that maybe the product needed to be re-saved so that the references to the new stock and source was created so found a product and tried to save it from the admin page, no that didn’t work either. 

The exception log told me that it was the database failing and that an reference of the source was failing, so I thought that re-adding the source to the product would work, unfortunately not. After a lot of debugging and investigations I finally removed the foreign key in the database and then the product could be saved.

Looking into the row at the database I noticed that the source that was added to the row was “default” and not the one I added, apparently there need to exist and source named exactly “default” so re-added the foreign key in the database and went back to the admin page and added the default source.

With the new source I tried to save a product again, and success the product was saved. Amazing, I could even see that the old quantity was still left from before the upgrade.

## Running a third test

So now we could save products again time to go back and try adding the product to the basket, opened a random product and tried adding it but no, the same error occurred. Not again, what have I missed, thinking about it for a minute I realized that maybe I should test with the product that was saved previously and almost success, no error but no products in the basket. So saving the products fixed the missing source problem but there is something else failing.

## Re-saving products

Tested saving the other product that could not be added into the basket previously and tried adding it again and could confirm that saving the products fixed that problem. But how can I save re-save that amount of products? That would take me ages, I took the decision to write an SQL script that generates the lines missing directly in the database.

```jsx
INSERT INTO inventory_source_item (source_code, sku, quantity, status)
SELECT 'default', cpe.sku, csi.qty, '1'
FROM catalog_product_entity as cpe
	JOIN cataloginventory_stock_item as csi ON cpe.entity_id = csi.product_id;
```

So with the missing lines in the database, I was hopping that both problems with no quantity and that it could not be placed in the basket was solved but unfortunately not, neither of the bugs was fixed. Still no quantity and no products could be added.

## With all inventory items added, let's try again

Let’s start with clearing the cache again and see if that worked, not this time. But wait, quantity is in the inventory and that needs to be indexed, running the indexer manually and clear the cache again.

```bash
php bin/magento indexer:reindex
```

And just like that the quantity was shown correctly again, great! But I still could not add the product to the basket and no error was shown in the storefront, what is happening?

## Continue searching for missing data

So after a long time I started thinking that somewhere there has to be an an table connecting the source with the stock, so I started digging into the database and found the `inventory_source_stock_link` table that was empty, looked over the references of the table and realized that there should be a link between the stock and the source here.

Took some time collecting what data should be added and then wrote down a SQL script and run it into the database.

```jsx
INSERT INTO `inventory_source_stock_link` (link_id, stock_id, source_code, priority)
VALUES ('1', '1', 'default', '1');
```

To be safe run an reindex again and cleared the cache and be hold! It was suddenly possible to place orders again and everything was back to normal.

## Conclusion

This was no easy task, especially for me that is no Magento expert. I am fortune that the owner of this site trust me enough to let me. Even if it was an complex task I had a lot of fun and won’t hesitate to do it again.