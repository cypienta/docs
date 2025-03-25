Start using Cypienta UI
===============

Once all your resources are deployed and the ECS app is up and in ``Running`` status. You can start using the Cypienta UI.


Start using Cypienta UI
-----------------------

1. Navigate to the CloudFormation stack that was created in the :doc:`deploy` page.

2. Select the stack that was created and click on the ``Outputs`` tab.

3. Note the Load balancer URL for the ``CypientaUI`` in the outputs section.

4. Open a new tab in your browser and navigate to the Load balancer URL.

5. Login to the portal with the default credentials.

    .. image:: resources/ui_login.png
        :alt: Login page
        :align: center
    
    .. note::
        The default ``Username`` is ``cypienta`` and the default ``Password`` is ``cypienta``

    .. image:: resources/home_page.png
        :alt: Home page
        :align: center


Explore Campaign List table
-------------------

1. On the left hand side panel, click on ``Clusters``. From the ``Clustering Agent Selection`` dropdown, you may select any of the clustering agents to get the list of campaigns for that clustering agent.

    .. image:: resources/campaign_list.png
        :alt: Campaign list
        :align: center

    
    .. note::
        The ``Clusters`` page will show all the campaigns that were created from the clustered events.
        The ``Flows`` page will show all the flows that were created from the events.


2. The Campaign List table will show the list of campaigns sorted in descending order of the campaign id. To sort it by ascending order, click on the campaign id column header. This will sort the campaigns in ascending order of the campaign id.

    .. image:: resources/campaign_list_sort.png
        :alt: Campaign list sort
        :align: center

    .. note::
        Any column header with a blue hyperlink can be clicked to sort the campaigns in ascending or descending order of that column.


3. You can also filter the campaigns by the ``Campaign Status`` column. Select the ``Campaign Status`` from the dropdown and click on the status you want to filter. This will filter the campaigns by the selected status.

    .. image:: resources/campaign_list_prefilter.png
        :alt: Campaign list pre filter
        :align: center
    

After filtering the campaigns, you can see the filtered campaigns in the table.

    .. image:: resources/campaign_list_postfilter.png
        :alt: Campaign list post filter
        :align: center


4. To hide columns from the table, click on the collapse icon on the top of the column header. This will hide the column from the table.

    .. image:: resources/campaign_list_precollapse.png
        :alt: Campaign list pre collapse
        :align: center

    
After collapsing the column, you can see the updated table as follows:

    .. image:: resources/campaign_list_postcollapse.png
        :alt: Campaign list post collapse
        :align: center

5. In the ``Assigned To`` colums, you can click on the dropdown and add new assignees to the campaigns.

    .. image:: resources/campaign_list_assign.png
        :alt: Campaign list pre assign
        :align: center

    You may also use the dropdown on the column header to filter the campaigns by the assigned user.


Explore Campaign page
---------------------

1. Once you click on a campaign ``Name`` from the Campaign List table, you will be redirected to the Campaign page. The Campaign page will show the current campaign status and different tabs to explore the campaign. The firs tab opened is the ``Playbook`` tab which shows a simga graph on the left hand side, a timeline on the right side and list of events per stage in the center.

    .. image:: resources/campaign_page.png
        :alt: Campaign page
        :align: center

    .. note::
        Select the ``Tactics tab`` to see the list of tactics for the campaign.
        Select the expand icon on the top right of the page to expand the center panel to include all possible columns.


2. To view a particular event, click on the event from the central panel. This will open a pop up with more details about the event.

    .. image:: resources/playbook_event.png
        :alt: Campaign page event
        :align: center
        

3. Click on the ``Events`` tab to see the list of events for the campaign.

    .. image:: resources/campaign_events.png
        :alt: Campaign page events
        :align: center


4. As similar to the you may collaps a column for the events table, and filter the events where there is a dropdown at the column header. Use the Raw Data button to expand the event details and view raw event data.

    .. image:: resources/campaign_raw_event.png
        :alt: Campaign page events raw
        :align: center


5. You can use the ``Hide`` button to hide an event from the campaign. This will open up a side panel, which will ask for a comment on why the event is being hidden. Click on ``Submit`` once the comment is added.

    .. image:: resources/campaign_hide_event.png
        :alt: Campaign page hide event
        :align: center
        
    You can see the hidden events in the ``Hidden Events`` section at the top of the page.

    .. image:: resources/campaign_hidden_events.png
        :alt: Campaign page hidden events
        :align: center

    You can see the Unhide button on the right side of the hidden event in the hidden events section. The number of hidden events, visible events is shown in the top of the page.


6. If you want to edit the TTPs for any event, click on the ``Feedback`` button. This will open up a side panel, where you can select the ``Tactics`` and ``Techniques`` for the event. You can also add a comment to the feedback. You may select the checkbox for ``Also update rows with the same message``, to modify the techniques and tactics for all such events with same message. You may also select the ``Use for training`` checkbox to use the feedback for training the model. Click on ``Submit`` once the changes are made.

    .. image:: resources/campaign_feedback.png
        :alt: Campaign page feedback
        :align: center
        

7. To select IOCs from the events, click on the ``IOCs`` button. This will open up a side panel, where you can select the IOCs from the events. For example, lets select the ``src`` as the field for IOC and add the type of IOC from the events.

    .. image:: resources/campaign_iocs_selection.png
        :alt: Campaign page IOCs
        :align: center
        
    .. note::
        You can see all the IOCs created for the campaign in the ``IOCs`` tab.


How to add comments to the campaign
-----------------------------------

1. Click on the ``Comments`` tab to see the list of comments for the campaign.

    .. image:: resources/campaign_comments.png
        :alt: Campaign page comments
        :align: center

    You can add a new comment by writing in the text area and clicking on ``Post Comment``.    


How to use "Cut Events" feature
-------------------------------

1. On the ``Playbook`` tab, central panel which shows the list of events, select the events you want to cut. Then click on the ``Actions`` dropdown and select ``Cut Events``.

    .. image:: resources/playbook_events_selection.png
        :alt: Playbook events selection
        :align: center

2. This will open a pop up panel as below.

    .. image:: resources/playbook_cut_events.png
        :alt: Playbook cut events
        :align: center

    This panel shows you the list of events, the source campaign.

3. Scroll down to see the list of campaigns that you want to cut the events to.

    .. image:: resources/playbook_cut_events_target_campaigns.png
        :alt: Playbook cut events target campaigns
        :align: center

    You can select one campaign you want to cut the events to by click on the add button in the ``Select Target Campaign`` section ``Available Campaigns``.
    
    .. image:: resources/playbook_cut_event_target_added.png
        :alt: Playbook cut events target campaigns
        :align: center

4. Write an optional comment for the cut events action you are performing. Use the switch ``Use this action for training the model`` to use the cut events action for training the model. Finally, click on ``Submit`` to perform the cut events action.

5. Click on ``OK`` to confirm the cut events action.

    .. image:: resources/playbook_cut_events_confirm.png
        :alt: Playbook cut events submit
        :align: center


6. You can see the events have been moved from the source campaign to the target campaign.

    .. image:: resources/playbook_cut_events_moved.png
        :alt: Playbook cut events moved
        :align: center


.. How to add Rules and Labels for campaigns
.. -----------------------------------------

.. 1. On the left hand side panel, click on ``Cluster`` drop down and select ``Rules``.

..     .. image:: resources/select_rules.png
..         :alt: select rules
..         :align: center

.. 2. Click on ``Add Rule`` button to add a new rule.

..     .. image:: resources/add_rule.png
..         :alt: add rule
..         :align: center

.. 3. Fill in the details for the rule. Give a distinguishable name to the rule. Select the metric on which you want to set a rule.
..    Select the condition and value for the rule. Do not select any of the campaigns in the ``Campaigns`` field and click on ``Save``.

..     .. image:: resources/new_rule.png
..         :alt: add rule details
..         :align: center

.. 4. Now to utilize the new rule we need to add a label to the campaign. On the left hand side panel, click on ``Cluster`` drop down and select ``Labels``.

..     .. image:: resources/select_labels.png
..         :alt: select labels
..         :align: center

.. 5. Click on ``Add Label`` button to add a new label.

..     .. image:: resources/add_label.png
..         :alt: add label
..         :align: center

.. 6. Fill in the details for the label. Give a distinguishable name to the label, which will be applied to all campaigns. Select the rules that you want to apply to the label and click on ``Save``.

..     .. image:: resources/new_label.png
..         :alt: add label details
..         :align: center

.. 7. Now go back to the ``Clusters`` page to see the list of Campaigns and you will see the label applied to all the campaigns.

..     .. image:: resources/view_labels.png
..         :alt: label applied
..         :align: center

..     .. note::
..         Applying new or edited rules or labels to all campaigns may take some time. Refresh the campaigns page to check if the changes have been applied.


.. Generate summary using Open AI
.. ------------------------------

.. 1. On the left hand side panel, click on ``GenAI``

..     .. image:: resources/gen_ai_add_key.png
..         :alt: gen ai config
..         :align: center

.. 2. Add your API key in the input field and click on ``Add API key``.

.. 3. On the left hand side panel, click on ``Campaigns``

..     .. image:: resources/campaign_list.png
..         :alt: Campaign list
..         :align: center

.. 4. Select any campaign for which you want to generate a summary. Click on ``Generate Summary`` button.

..     .. image:: resources/gen_ai_create_summary.png
..         :alt: gen ai summary
..         :align: center

.. 4. Click on the ``Diamond`` tab and view the summary created for your selected campaign.

..     .. image:: resources/gen_ai_summary.png
..         :alt: gen ai summary
..         :align: center