<script setup lang="ts">
import { ref,toRaw,onMounted } from 'vue'
import { Univer, UniverInstanceType, Workbook, LocaleType,FUniver,Range,SheetTypes } from "@univerjs/core";
import { defaultTheme } from "@univerjs/design";
import { UniverUIPlugin } from "@univerjs/ui";
import { UniverDocsPlugin } from "@univerjs/docs";
import { UniverDocsUIPlugin } from "@univerjs/docs-ui";
import { UniverSheetsPlugin } from "@univerjs/sheets";
import { UniverSheetsUIPlugin } from "@univerjs/sheets-ui";
import { UniverSheetsFilterPlugin } from '@univerjs/sheets-filter';
import { UniverSheetsFilterUIPlugin } from '@univerjs/sheets-filter-ui';
import { UniverSheetsDataValidationPlugin } from '@univerjs/sheets-data-validation';
import { UniverSheetsDataValidationUIPlugin } from '@univerjs/sheets-data-validation-ui';

import '@univerjs/ui/facade';
import '@univerjs/sheets/facade';
import '@univerjs/docs-ui/facade';
import '@univerjs/sheets-ui/facade';
import '@univerjs/sheets-data-validation/facade';

import { zhCN, enUS } from 'univer:locales'

defineProps<{ msg: string }>()

const univerSheetRef = ref()
const univerAPI = ref(null)
const univerRef = ref(null)

onMounted(()=>{
  init()
})

function init(){
  const univer = new Univer({
    theme: defaultTheme,
    locale: LocaleType.ZH_CN,
    locales: {
      [LocaleType.ZH_CN]: zhCN,
      [LocaleType.EN_US]: enUS,
    },
  });
  univerRef.value = univer;
  univer.registerPlugin(UniverUIPlugin, {
    container: univerSheetRef.value,
    // toolbar:false,
    // header:false,
    footer:false
  });
  univer.registerPlugin(UniverDocsPlugin, {
    hasScroll: false,
  });
  univer.registerPlugin(UniverDocsUIPlugin);
  univer.registerPlugin(UniverSheetsPlugin);
  univer.registerPlugin(UniverSheetsUIPlugin);
  univer.registerPlugin(UniverSheetsFilterPlugin);
  univer.registerPlugin(UniverSheetsFilterUIPlugin);
  univer.registerPlugin(UniverSheetsDataValidationPlugin);
  univer.registerPlugin(UniverSheetsDataValidationUIPlugin, {
    // 是否在下拉菜单中显示编辑按钮
    showEditOnDropdown: true
  });

  univer.createUnit(UniverInstanceType.UNIVER_SHEET,{
    id:`test-workbook`,
    appVersion:'0.5.1',
    name:`测试`,
    locale: LocaleType.ZH_CN,
    sheets:{
      'test-sheet1':{
        type: SheetTypes.GRID,
        id:`test-sheet1`,
        name:'普通报表',
        columnCount:52
      }
    }
  })
  univerAPI.value = FUniver.newAPI(toRaw(univerRef.value));
}

</script>

<template>
  <div ref="univerSheetRef" class="container"></div>
</template>

<style scoped>
.container {
 width: 100%;
 height: 100%;
}
</style>
