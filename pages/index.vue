<template>
  <div class="container">
    <div>
      <h1 class="title">Exportar a CSV</h1>
      <div class="links">
        <button @click="downloadCSV" class="button--green">Decargar CSV</button>
      </div>
    </div>
  </div>
</template>

<script>
import data from "~/mock/data.json";

export default {
  methods: {
    getCSVHeaders() {
      let headers = "";
      this.csvColumns.forEach(csvColumn => {
        headers += `${csvColumn.value}${this.csvSeparator}`;
      });
      return headers;
    },
    getCSVBody() {
      let body = "";
      this.csvRows.forEach(csvRow => {
        this.csvColumns.forEach(csvColumn => {
          body += `${csvRow[csvColumn.key]}${this.csvSeparator}`;
        });
        body += this.csvBreakLine;
      });
      return body;
    },
    getCSVContent() {
      let csvHeaders = this.getCSVHeaders();
      let csvBody = this.getCSVBody();

      let csvContent = "";
      csvContent += `${this.universalBOM}`;
      csvContent += `${csvHeaders}${this.csvBreakLine}`;
      csvContent += csvBody;
      csvContent += `sep=${this.csvSeparator}${this.csvBreakLine}`;

      return csvContent;
    },
    downloadCSV() {
      let csvContent = this.getCSVContent();
      let fileName = this.fileName;
      let mimeType = this.csvMimeType;

      var blob = new Blob([csvContent], { type: mimeType });
      if (navigator.msSaveBlob) {
        navigator.msSaveBlob(blob, fileName);
      } else {
        var link = document.createElement("a");
        if (link.download === undefined) return;

        var url = URL.createObjectURL(blob);
        link.setAttribute("href", url);
        link.setAttribute("download", fileName);
        link.style.visibility = "hidden";
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      }
    }
  },
  computed: {
    universalBOM() {
      return "\uFEFF";
    },
    fileName() {
      return "Products.csv";
    },
    csvMimeType() {
      return "text/csv;charset=utf-8;";
    },
    csvSeparator() {
      return ";";
    },
    csvBreakLine() {
      return "\n";
    },
    csvColumns() {
      return [
        {
          key: "dutyCode",
          value: "Arancel"
        },
        {
          key: "brandCode",
          value: "Marca"
        },
        {
          key: "modelCode",
          value: "Modelo"
        },
        {
          key: "productCode",
          value: "Producto"
        },
        {
          key: "countryCode",
          value: "País de Origen"
        },
        {
          key: "unitCode",
          value: "Unidad de Medida"
        },
        {
          key: "weight",
          value: "Peso"
        },
        {
          key: "quantity",
          value: "Cantidad"
        },
        {
          key: "fobAmount",
          value: "Valor FOB"
        },
        {
          key: "registerNumber",
          value: "No. de Registro"
        },
        {
          key: "lotNumber",
          value: "No. de Lote"
        },
        {
          key: "specification",
          value: "Especificación"
        },
        {
          key: "description",
          value: "Descripción"
        }
      ];
    },
    csvRows() {
      return data;
    }
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
