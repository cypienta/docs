Start using Cypienta UI
===============

Once all your resources are deployed and the ECS app is up and in ``Running`` status. You can start using the Cypienta UI.

Start using Cypienta UI
-----------------------

1. Note the Load balancer URL for the UI that is deployed. Go to AWS
   Console and search for ``EC2`` and on the left hand side panel, expand
   ``Load Balancing``, and select ``Load Balancers``. Copy the ``DNS name`` of
   the corresponding load balancer and keep note of it.

    .. image:: resources/dns_name.png
        :alt: get dns name
        :align: center

2. Open your browser with URL: ``http://<DNS-name>:8000``.

    .. image:: resources/ui_login.png
        :alt: Login to UI
        :align: center

3. Login to the portal.

    .. image:: resources/home_page.png
        :alt: Home page
        :align: center
    
    .. note::
        The default ``Username`` is ``maestro`` and the default ``Password`` is ``changemenow``


How to use the Hide feature for events in UI
----------------------------------------------

1. On the left hand side panel, click on ``Campaigns``

    .. image:: resources/campaign_list.png
        :alt: Campaign list
        :align: center

2. Click on any campaign that you want to modify:

    .. image:: resources/hide_open_campaign.png
        :alt: open campaign
        :align: center

3. Select the events from the ``Stages`` panel by clicking on the radio
   button on the top right of each event

    .. image:: resources/hide_select_event.png
        :alt: select event
        :align: center

4. Click on the edit icon (one with the pencil) on top of the panel and
   click on ``Hide`` to hide the events from the campaign.

    .. image:: resources/click_hide.png
        :alt: click hide
        :align: center

5. Click on the ``Events`` tab in the analysis page to see a list of all
   events under the selected campaign

    .. image:: resources/hide_events_tab.png
        :alt: click hide
        :align: center

   You can see the list of hidden events now has an event that was selected earlier and hide action was taken.

Edit recognized techniques for events
-------------------------------------

1. On the left hand side panel, click on ``Campaigns``

    .. image:: resources/campaign_list.png
        :alt: Campaign list
        :align: center

2. Click on any campaign that you want to modify:

    .. image:: resources/tech_campaign.png
        :alt: Open campaign
        :align: center

3. Select the ``Events`` tab to see the list of events under the selected
   campaign

    .. image:: resources/tech_events_tab.png
        :alt: events tab
        :align: center

4. Click on the Feedback button on the right-hand side for the event
   that you want to modify. Select the appropriate tactics and
   techniques that you want for the event and add an optional comment
   for the feedback. Select the check box ``Also update rows with the
   same message``, to modify the techniques and tactics for all such
   events with same message. Click on ``Submit`` to record the feedback on
   the UI.

    .. image:: resources/tech_feedback.png
        :alt: feedback
        :align: center


How to use "Cut Events" feature
-------------------------------

1. On the left hand side panel, click on ``Campaigns``

    .. image:: resources/campaign_list.png
        :alt: Campaign list
        :align: center

2. Click on any campaign from which you want to copy events:

   Picking two campaigns for reference below:

    .. image:: resources/cut_show_campaigns.png
        :alt: show campaigns
        :align: center

   Notice the event count for campaign name ``3599.0`` is 4 and ``3572.0`` is 12. 

   Click on campaign name ``3599.0`` to cut the event to ``3572.0``

3. Select the event that you want to cut and click on the edit button
   on top of the section

    .. image:: resources/cut_select_event.png
        :alt: select event
        :align: center

4. Select ``Cut Events``, and a side panel will appear to select campaign(s) you want to cut the events to. Search for the ``campaign id`` or the ``campaign name`` you want to cut events to. Here, selection shows campaign name ``3614``, campaign id ``23``. You may choose different values for the ``Event attributes`` and ``Node attributes``. Default value for all the attribute is set to 0. And click on ``Done``.

    .. image:: resources/cut_modal.png
        :alt: cut modal
        :align: center

5. Click on ``OK`` button to confirm your cut action

6. Navigate back to the Campaigns page. You will notice now that the campaign name ``3599.0`` has an event count of 3, changed from 4. And the ``3572.0`` campaign has an event count of 13 changed from 12. 

    .. image:: resources/cut_completed.png
        :alt: cut completed
        :align: center