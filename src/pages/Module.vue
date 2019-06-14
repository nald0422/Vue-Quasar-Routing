<template>
  <div class="q-pa-md">
    <q-layout view="lHh lpr lFf" container style="height: 400px" class="shadow-2 rounded-borders">
      <q-header elevated>
        <q-bar>
          <q-icon name="laptop_chromebook"/>
          <div>Google Chrome</div>

          <q-space/>

          <q-btn dense flat icon="minimize"/>
          <q-btn dense flat icon="crop_square"/>
          <q-btn dense flat icon="close"/>
        </q-bar>

        <div class="q-pa-sm q-pl-md row items-center">
          <div class="cursor-pointer non-selectable">
            <q-icon name="arrow_back" color="positive" size="16px"/>Previous
            <q-menu>
              <q-list dense style="min-width: 100px">
                <q-item clickable v-close-popup>
                  <q-item-section>Open...</q-item-section>
                </q-item>
                <q-item clickable v-close-popup>
                  <q-item-section>New</q-item-section>
                </q-item>

                <q-separator/>

                <q-item clickable>
                  <q-item-section>Preferences</q-item-section>
                  <q-item-section side></q-item-section>

                  <q-menu anchor="top right" self="top left">
                    <q-list>
                      <q-item v-for="n in 3" :key="n" dense clickable>
                        <q-item-section>Submenu Label</q-item-section>
                        <q-item-section side>
                          <q-icon name="keyboard_arrow_right"/>
                        </q-item-section>
                        <q-menu auto-close anchor="top right" self="top left">
                          <q-list>
                            <q-item v-for="n in 3" :key="n" dense clickable>
                              <q-item-section>3rd level Label</q-item-section>
                            </q-item>
                          </q-list>
                        </q-menu>
                      </q-item>
                    </q-list>
                  </q-menu>
                </q-item>

                <q-separator/>

                <q-item clickable v-close-popup>
                  <q-item-section>Quit</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="arrow_forward" color="positive" margin-right="5px;"/>Next
            <q-menu auto-close>
              <q-list dense style="min-width: 100px">
                <q-item clickable>
                  <q-item-section>Cut</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Copy</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Paste</q-item-section>
                </q-item>
                <q-separator/>
                <q-item clickable>
                  <q-item-section>Select All</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </div>

          <div class="q-ml-lg cursor-pointer non-selectable">
            <q-icon name="save" size="16px"/>Save
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="print"/>Print
            <q-menu auto-close>
              <q-list dense style="min-width: 100px">
                <q-item clickable>
                  <q-item-section>Printer A</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Printer B</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Printer C</q-item-section>
                </q-item>
                <q-separator/>
              </q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="email"/>E-mail
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
            </q-menu>
          </div>
          <q-separator vertical class="q-ml-md"/>
          <div class="q-ml-sm cursor-pointer non-selectable">
            <q-icon name="find_in_page"/>Find
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="spellcheck"/>Spelling
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="history"/>History
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="note"/>Journal
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            <q-icon name="edit"/>Customize
            <q-menu auto-close>
              <q-list dense style="min-width: 100px"></q-list>
              <q-list dense style="min-width: 100px">
                <q-item clickable>
                  <q-item-section>Printer A</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Printer B</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Printer C</q-item-section>
                </q-item>
                <q-separator/>
              </q-list>
            </q-menu>
          </div>
          <q-separator vertical class="q-ml-md"/>
          <div class="q-ml-sm cursor-pointer non-selectable">
            <q-icon name="attach_file"/>Attach
            <q-menu auto-close></q-menu>
          </div>
        </div>
      </q-header>

      <q-page-container>
        <q-page class="q-pa-md">
          <div class="row">
            <div class="col-sm-3">
              <q-btn-dropdown color="primary" label="Vendor" size="12px" glossy>
                <q-list>
                  <q-item clickable v-close-popup>
                    <q-item-section>
                      <q-item-label>Vendor A</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-item clickable v-close-popup>
                    <q-item-section>
                      <q-item-label>Vendor B</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-item clickable v-close-popup>
                    <q-item-section>
                      <q-item-label>Vendor C</q-item-label>
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-btn-dropdown>
            </div>
            <div class="col-sm-3">
              <q-btn-dropdown color="primary" label="Class" size="12px" glossy>
                <q-list>
                  <q-item clickable v-close-popup>
                    <q-item-section>
                      <q-item-label>Class A</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-item clickable v-close-popup>
                    <q-item-section>
                      <q-item-label>Class B</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-item clickable v-close-popup>
                    <q-item-section>
                      <q-item-label>Class C</q-item-label>
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-btn-dropdown>
            </div>
            <div class="col-sm-6">
              <div class="row">
                <div class="col-7"></div>
                <div class="col-sm-5" style="text-align: right">
                  <div
                    class="col-xs-12 col-md-6 col-sm-8 col-lg-6 center-block"
                    style="height: 35px;"
                  >
                    <p>
                      Template
                      <q-btn color="purple" size="12px">Print Preview</q-btn>
                    </p>
                    <br>
                  </div>
                  <div class="col-lg-1">
                    <q-btn-dropdown
                      color="primary"
                      label="Custom Purchase Order"
                      size="12px"
                      glossy
                    >
                      <q-list>
                        <q-item clickable v-close-popup>
                          <q-item-section>
                            <q-item-label>Class A</q-item-label>
                          </q-item-section>
                        </q-item>

                        <q-item clickable v-close-popup>
                          <q-item-section>
                            <q-item-label>Class B</q-item-label>
                          </q-item-section>
                        </q-item>

                        <q-item clickable v-close-popup>
                          <q-item-section>
                            <q-item-label>Class C</q-item-label>
                          </q-item-section>
                        </q-item>
                      </q-list>
                    </q-btn-dropdown>
                  </div>
                </div>
              </div>
            </div>
            <!-- <div class="row">
              <div class="col-sm-12">
                <q-btn-dropdown color="primary" label="Vendor" size="12px" glossy>
                  <q-list>
                    <q-item clickable v-close-popup>
                      <q-item-section>
                        <q-item-label>Vendor A</q-item-label>
                      </q-item-section>
                    </q-item>

                    <q-item clickable v-close-popup>
                      <q-item-section>
                        <q-item-label>Vendor B</q-item-label>
                      </q-item-section>
                    </q-item>

                    <q-item clickable v-close-popup>
                      <q-item-section>
                        <q-item-label>Vendor C</q-item-label>
                      </q-item-section>
                    </q-item>
                  </q-list>
                </q-btn-dropdown>
              </div>
            </div>-->
          </div>
          <hr>
          <div class="row">
            <p>Purchase Requisition</p>
          </div>
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>