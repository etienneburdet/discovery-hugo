---
title: "Refine on Click"
date: 2020-08-10T14:42:01+02:00
draft: true
---

# Use a map as a selector and display results on the side

This template propose a 2 steps dynamic view. First you propose a map with clickable points or shapes.
- Global information are displayed on the side.
- Then, the user clic on the map, it then open a drawer with specific information relative to the selection.
- Finally, if the user wants to know more about this selection, a second drawer expend to take almost all the screen width.

This template is the best solution when you desire to display a lot of information and that just can't fit in the default tooltip !
N.B.: grey areas help the user to come back to the map quickly if they want to change their selection

{{< highlight js >}}
<div class="ods-box donotcopy-specific"
     ng-init="donotcopy = { 'simulaterefineonclick' : false}">
    <div class="map-drawer-container"
         ng-class="{
                   'map-drawer-container--active': donotcopy.simulaterefineonclick,
                   'map-drawer-container--active-full': drawer.status}"
         ng-init="drawer = {'status':false}">    
        <div class="map-drawer-container__map">
            <div class="map-drawer-container__backdrop"
                 ng-click="donotcopy.simulaterefineonclick = undefined;
                           drawer.status = false"
                 ng-class="{'map-drawer-container__backdrop--active': donotcopy.simulaterefineonclick }">
            </div>
            <h1>
                Map
            </h1>
            <div class="ods-button"
                 ng-click="donotcopy.simulaterefineonclick = true">Refine-on-click</div>
        </div>
        <div class="map-drawer-container__info">
            <h2>
                General info here
            </h2>
        </div>
        <div class="map-drawer-container__drawer map-drawer-container__drawer__partial">                      
            <div class="map-drawer-container__drawer__close"
                 ng-click="donotcopy.simulaterefineonclick = undefined;
                           drawer.status = false">
                <i class="fa fa-times"></i>
            </div>
            <h2>
                Specific info here
            </h2>
            <div class="ods-button"
                 ng-click="drawer.status = !drawer.status">
                Expend / See more
            </div>
        </div>
        <div class="map-drawer-container__drawer map-drawer-container__drawer__full">
            <h2>
                More info here
            </h2>
        </div>
    </div>
</div>
{{< / highlight >}}
