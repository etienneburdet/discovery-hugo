---
title: "Refine on Click"
date: 2020-08-10T14:42:01+02:00
draft: false
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
{{< ods-page >}}
<ods-dataset-context context="shanghai,shanghaihistoric"
                     shanghai-dataset="shanghai-world-university-ranking"
                     shanghai-domain="fpassaniti"
                     shanghaihistoric-dataset="shanghai-world-university-ranking"
                     shanghaihistoric-domain="fpassaniti"
                     shanghaihistoric-parameters="{'sort':'year'}">
    <div class="ods-box">
        <h2>
            Shanghai World University Ranking
        </h2>
        <div class="map-drawer-container"
             ng-class="{
                       'map-drawer-container--active': shanghai.parameters['refine.country'],
                       'map-drawer-container--active-full': drawer.status}"
             ng-init="drawer = {'status':false}">    
            <div class="map-drawer-container__map">
                <div class="map-drawer-container__backdrop"
                     ng-click="shanghai.parameters['refine.country'] = undefined;
                               shanghaihistoric.parameters['refine.university_namex'] = undefined;
                               drawer.status = false"
                     ng-class="{'map-drawer-container__backdrop--active': shanghai.parameters['refine.country'] }">
                </div>
                <ods-map no-refit="false"
                         scroll-wheel-zoom="true"
                         display-control="false"
                         toolbar-fullscreen="false"
                         toolbar-drawing="false"
                         toolbar-geolocation="false"
                         location="2,33.39542,4.10444">
                    <ods-map-layer context="shanghai"
                                   display="choropleth"
                                   function="AVG"
                                   expression="world_rank_integer"
                                   color-numeric-ranges="{'120':'#000000','175':'#2E3539','225':'#5D6A73','350':'#8C9FAD','500':'#BBD5E7'}"
                                   color-undefined="#FFFFFF"
                                   color-out-of-bounds="#FFFFFF"
                                   color-numeric-range-min="1"
                                   shape-opacity="0.9"
                                   caption="false"
                                   refine-on-click-context="[shanghai,shanghaihistoric]"
                                   refine-on-click-shanghai-context-field="country"
                                   refine-on-click-shanghai-map-field="country"
                                   refine-on-click-shanghai-replace-refine="true"
                                   refine-on-click-shanghaihistoric-context-field="country"
                                   refine-on-click-shanghaihistoric-map-field="country"
                                   refine-on-click-shanghaihistoric-replace-refine="true">
                    </ods-map-layer>
                </ods-map>
            </div>
            <div class="map-drawer-container__info">
                <h2>
                    Pick a country on the map
                </h2>
            </div>
            <div class="map-drawer-container__drawer map-drawer-container__drawer__partial">                      
                <div class="map-drawer-container__drawer__close"
                     ng-click="shanghai.parameters['refine.country'] = undefined;
                               shanghaihistoric.parameters['refine.university_namex'] = undefined;
                               drawer.status = false">
                    <i class="fa fa-times"></i>
                </div>
                <h2>
                    {{ shanghai.parameters['refine.country'] }} universities
                </h2>
                <h3>
                    sorted by average ranking
                </h3>
                <div ods-analysis="univs"
                     ods-analysis-context="shanghai"
                     ods-analysis-x="university_name"
                     ods-analysis-max="0"
                     ods-analysis-serie-agg="AVG(world_rank_integer)"
                     ods-analysis-sort="-agg">
                    <ul>
                        <li class="univ">
                            <div class="univ-name">
                                University
                            </div>
                            <div class="univ-score">
                                Avg. rank
                            </div>
                            <div class="univ-seemore">
                                <i class="fa fa-line-chart" aria-hidden="true"></i>
                            </div>
                        <li ng-repeat="univ in univs.results"
                            class="univ">
                            <div class="univ-name"
                                 ng-class="{'univ-selected': shanghaihistoric.parameters['refine.university_name'] == univ.x}">
                                {{ univ.x }}
                            </div>
                            <div class="univ-score">
                                {{ univ.agg | number : 0 }}
                            </div>
                            <div class="univ-seemore"
                                 ng-click="shanghaihistoric.parameters['refine.university_name'] = univ.x;
                                           drawer.status = true">
                                <i class="fa fa-line-chart" aria-hidden="true"></i>
                            </div>
                        </li>
                    </ul>                        
                </div>
            </div>
            <div class="map-drawer-container__drawer map-drawer-container__drawer__full">
                <h2>
                    {{ shanghaihistoric.parameters['refine.university_name'] }} ({{ shanghai.parameters['refine.country'] }})
                </h2>
                <ods-tabs layout="simple-nav" pane-auto-unload="true">
                    <ods-pane title="Chart">
                        <ods-chart timescale="year" display-legend="false" align-month="false">
                            <ods-chart-query context="shanghaihistoric" field-x="year" maxpoints="200" timescale="year">
                                <ods-chart-serie expression-y="world_rank_integer" chart-type="column" function-y="AVG" label-y="Ranking range per year" color="#142E7B" display-units="true" scientific-display="true">
                                </ods-chart-serie>
                            </ods-chart-query>
                        </ods-chart>
                    </ods-pane>
                    <ods-pane title="Table" pane-auto-unload="true">
                        <ods-table context="shanghaihistoric"
                                   displayed-fields="world_rank,national_rank,year"></ods-table>
                    </ods-pane>
                </ods-tabs>
            </div>
        </div>
    </div>
</ods-dataset-context>
{{< / ods-page >}}
