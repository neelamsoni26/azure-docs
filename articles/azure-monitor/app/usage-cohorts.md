---
title: Application Insights usage cohorts | Microsoft Docs
description: Analyze different sets or users, sessions, events, or operations that have something in common.
ms.topic: conceptual
ms.date: 07/30/2021
---

# Application Insights cohorts

A cohort is a set of users, sessions, events, or operations that have something in common. In Application Insights, cohorts are defined by an analytics query. In cases where you have to analyze a specific set of users or events repeatedly, cohorts can give you more flexibility to express exactly the set you're interested in.

## Cohorts vs. basic filters

You can use cohorts in ways similar to filters. But cohorts' definitions are built from custom analytics queries, so they're much more adaptable and complex. Unlike filters, you can save cohorts so that other members of your team can reuse them.

You might define a cohort of users who have all tried a new feature in your app. You can save this cohort in your Application Insights resource. It's easy to analyze this saved group of specific users in the future.
> [!NOTE]
> After cohorts are created, they're available from the Users, Sessions, Events, and User Flows tools.

## Example: Engaged users

Your team defines an engaged user as anyone who uses your app five or more times in a given month. In this section, you define a cohort of these engaged users.

1. Select **Create a Cohort**.
1. Select the **Template Gallery** tab to see a collection of templates for various cohorts.
1. Select **Engaged Users -- by Days Used**.

    There are three parameters for this cohort:
    * **Activities**: Where you choose which events and page views count as usage.
    * **Period**: The definition of a month.
    * **UsedAtLeastCustom**: The number of times users need to use something within a period to count as engaged.

1. Change **UsedAtLeastCustom** to **5+ days**. Leave **Period** set as the default of 28 days.

    Now this cohort represents all user IDs sent with any custom event or page view on 5 separate days in the past 28 days.

1. Select **Save**.

   > [!TIP]
   > Give your cohort a name, like *Engaged Users (5+ Days)*. Save it to *My reports* or *Shared reports*, depending on whether you want other people who have access to this Application Insights resource to see this cohort.

1. Select **Back to Gallery**.

### What can you do by using this cohort?

Open the Users tool. In the **Show** dropdown box, choose the cohort you created under **Users who belong to**.

:::image type="content" source="./media/usage-cohorts/cohort-2.png" alt-text="Screenshot that shows the Show dropdown showing a cohort.":::

Important points to notice:

* You can't create this set through normal filters. The date logic is more advanced.
* You can further filter this cohort by using the normal filters in the Users tool. Although the cohort is defined on 28-day windows, you can still adjust the time range in the Users tool to be 30, 60, or 90 days.

These filters support more sophisticated questions that are impossible to express through the query builder. An example is _people who were engaged in the past 28 days. How did those same people behave over the past 60 days?_

## Example: Events cohort

You can also make cohorts of events. In this section, you define a cohort of events and page views. Then you see how to use them from the other tools. This cohort might define a set of events that your team considers _active usage_ or a set related to a certain new feature.

1. Select **Create a Cohort**.
1. Select the **Template Gallery** tab to see a collection of templates for various cohorts.
1. Select **Events Picker**.
1. In the **Activities** dropdown box, select the events you want to be in the cohort.
1. Save the cohort and give it a name.

## Example: Active users where you modify a query

The previous two cohorts were defined by using dropdown boxes. You can also define cohorts by using analytics queries for total flexibility. To see how, create a cohort of users from the United Kingdom.

1. Open the Cohorts tool, select the **Template Gallery** tab, and select **Blank Users cohort**.

   :::image type="content" source="./media/usage-cohorts/cohort.png" alt-text="Screenshot that shows the template gallery for cohorts." lightbox="./media/usage-cohorts/cohort.png":::

    There are three sections:

   * **Markdown text**: Where you describe the cohort in more detail for other members on your team.
   * **Parameters**: Where you make your own parameters, like **Activities**, and other dropdown boxes from the previous two examples.
   * **Query**: Where you define the cohort by using an analytics query.

     In the query section, you [write an analytics query](/azure/kusto/query). The query selects the certain set of rows that describe the cohort you want to define. The Cohorts tool then implicitly adds a `| summarize by user_Id` clause to the query. This data appears as a preview underneath the query in a table, so you can make sure your query is returning results.

     > [!NOTE]
     > If you don't see the query, resize the section to make it taller and reveal the query.

1. Copy and paste the following text into the query editor:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

1. Select **Run Query**. If you don't see user IDs appear in the table, change to a country/region where your application has users.

1. Save and name the cohort.

## Frequently asked question

### I defined a cohort of users from a certain country/region. When I compare this cohort in the Users tool to setting a filter on that country/region, why do I see different results?

Cohorts and filters are different. Suppose you have a cohort of users from the United Kingdom (defined like the previous example), and you compare its results to setting the filter `Country or region = United Kingdom`:

* The cohort version shows all events from users who sent one or more events from the United Kingdom in the current time range. If you split by country or region, you likely see many countries and regions.
* The filters version only shows events from the United Kingdom. If you split by country or region, you see only the United Kingdom.

## Learn more

* [Analytics query language](../logs/log-analytics-tutorial.md?toc=%2fazure%2fazure-monitor%2ftoc.json)
* [Users, sessions, events](usage-segmentation.md)
* [User flows](usage-flows.md)
* [Usage overview](usage-overview.md)
