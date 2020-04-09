<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
      right
      width="450"
    >
      <v-toolbar>
        <v-icon medium class="tune">mdi-tune</v-icon>
        <v-toolbar-title>Filtros</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn icon class="hidden-xs-only" @click.stop="drawer = !drawer">
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </v-toolbar>

      <p>Utilize os filtros abaixo para refinar os resultados da tabela, clicque no botão APLICAR para salvar as alterações.</p>

      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-calendar</v-icon>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title>Todas as datas de inclusão</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-calendar</v-icon>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title>Todas as datas de alteração</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-music-note-whole-dotted</v-icon>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title>Ativos e inativos</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <div class="apply">
        <v-btn outlined x-large>Aplicar</v-btn>
      </div>
    </v-navigation-drawer>

    <v-app-bar
      app
      style="background:#f5f5f5"
      dark
    >
      <div class="left">
        <div class="group">
          <v-btn light depressed class="white">
            <v-icon small>mdi-security</v-icon>
          </v-btn>
          <v-btn light depressed class="colorful">
            <v-icon small>mdi-account</v-icon>
          </v-btn>
        </div>
        <v-text-field
          append-icon="mdi-magnify"
          class="mx-0"
          background-color="rgb(245, 245, 245)"
          color="rgba(0, 0, 0, 0.54)"
          full-width
          light
          dense
          style="width:50%"
          flat
          hide-details
          label="Pesquisar..."
          solo></v-text-field>
      </div>

      <div class="right">
        <v-btn depressed class="btn secundary"><v-icon medium @click.stop="drawer = !drawer">mdi-tune</v-icon></v-btn>
        <v-btn depressed class="btn" large><v-icon small>mdi-account</v-icon><span>Incluir usuário</span></v-btn>
        <ul class="menu-main">
          <li><a href="#"><v-icon medium>mdi-home</v-icon></a></li>
          <li><a href="#"><v-icon medium>mdi-cog</v-icon></a></li>
          <li><a href="#"><v-icon medium>mdi-power-standby</v-icon></a></li>
        </ul>
      </div>
    </v-app-bar>

    <v-content>
      <v-container class="fill-height" fluid>
        <v-row
          align="center"
          justify="center"
        >
          <v-col class="text-center">

            <v-data-table
              v-model="selected"
              :page.sync="page"
              :items-per-page="itemsPerPage"
              hide-default-footer
              @page-count="pageCount = $event"
              :single-select="singleSelect"
              item-key="id"
              show-select
              :headers="headers"
              :items="desserts"
              class="elevation-1"
              style="text-align:center"
            >

              <template v-slot:item.status="{ item }">
                <v-chip :color="getColor(item.status)" dark>{{ item.status }}</v-chip>
              </template>
              
              <template v-slot:item.txt><b>...</b></template>

              <template v-slot:item.actions="{ item }">
                <span class="crud-actions">
                  <v-icon small @click="deleteItem(item)">
                    mdi-delete
                  </v-icon>

                  <v-icon small>
                    mdi-package-down
                  </v-icon>

                  <v-icon small>
                    mdi-security
                  </v-icon>
                  
                  <v-icon>
                    mdi-pencil
                  </v-icon>
                </span>
              </template>
            
            </v-data-table>
            
            <v-pagination
            color="#d83367" v-model="page" :length="pageCount"></v-pagination>

          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer
      color="#333333"
      app
    >
      <span class="date">{{currentlyDate(str_date)}}</span>
      <span class="white--text">&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<style>
  header{color:#757575!important}

  footer{justify-content: flex-end;}

  table{overflow:hidden; position: relative}
  table tr{position: relative;}
  table tr:nth-child(even) {background: #e9e9e9}
  table tr:nth-child(odd) {background: #f5f5f5}
  table tr:hover .crud-actions {opacity: 1;}
  table th{text-transform: uppercase;text-align: center!important;}
  table td b{font-size:22px;display: block;text-align: center;}
  table thead tr{background: #fff!important}
  table tbody tr:hover td,
  table tbody td:hover{background-color:#fff;border-bottom: solid 2px #d83367!important;color:#ddd!important}
  table tbody tr:hover td .active,
  table tbody tr:hover td .inactive{color:#ddd!important}
  table .mdi-checkbox-blank-outline{color:#ddd!important}

  button.white{background:#fff;}
  button.colorful{background: rgb(225,73,73);background: linear-gradient(90deg, rgba(225,73,73,1) 0%, rgba(245,115,21,1) 100%)!important;margin-left:-4px}
  button.colorful i{color:#fff!important}

  .apply{padding:0 16px;margin-top:24px} 
  .apply button{text-transform: uppercase;    color: #d83367!important;opacity:.25;width:100%}

  .menu-main{list-style:none;display: flex;}
  .menu-main::before{display:block;content:'';border-left:solid 1px #e9e9e9;padding-right:12px}
  .menu-main li{margin-left: 5px;}
  .menu-main a{text-decoration: none;}
  .menu-main a i {color:#757575!important}

  .v-navigation-drawer{background-color: rgb(245, 245, 245)!important;}
  .v-navigation-drawer .v-toolbar__title{font-weight:bold}
  .v-navigation-drawer__content p{padding: 0 16px;font-style: italic;margin:24px 0;font-size: 15px;color:rgba(0, 0, 0, 0.6)}
  .v-navigation-drawer__content header{text-transform:uppercase;background:#fff}
  .v-navigation-drawer__content header .tune{margin-right:8px}
  .v-list-item__title{text-transform: uppercase;color:#d83367;font-size:16px!important}

  .btn{background:#d83367!important;text-transform: uppercase;}
  .btn i{margin-right:6px}
  .btn.secundary{background-color:#fff!important;color:#757575!important}

  .crud-actions {opacity: 0;transition: 0.2s ease-in-out;}
  .crud-actions button{font-size:22px!important;margin-left:5px}
  .crud-actions b{font-weight: bold;font-size: 18px;}

  .left{width: 50%;display: flex;align-items: center;justify-content: space-around;;}
  .right{display: flex;align-items: center;justify-content: flex-end;width: 50%}

  .inactive{background: none!important;color:#d83367!important;text-transform: uppercase;font-weight: bold;}
  .active{background: none!important;color:#8ed184!important;text-transform: uppercase;font-weight: bold;}

  .date{font-size: 12px;font-style: italic;color:#fff;margin-right:12px}

  .v-application--wrap{background: rgb(245, 245, 245);}
  .v-content{flex:initial}
  .v-input{flex:initial!important}
  .v-input__slot{border-bottom: solid 2px #ddd!important;padding: 0!important;border-radius: initial!important;}
  .v-label {font-size: 14px!important;font-style: italic!important;}
  .v-toolbar{box-shadow: 0px 0px 20px #ccc!important;}

  @media only screen and (max-width: 600px) {
    header .v-input{width: 25%!important;}
    header .btn,
    .colorful {padding: 0!important;min-width: 44px!important;}
    table tbody tr:hover td,
    table tbody td:hover{background-color:none;border-bottom:none!important;color:#ddd!important}
    .v-btn__content span{display: none!important;}
    .v-toolbar__content, .v-toolbar__extension{padding: 0!important;}
    .v-navigation-drawer__content .v-toolbar__content{padding:4px 16px!important}
    .v-application button.hidden-xs-only{display:block!important}
    .v-navigation-drawer{position: fixed!important;}
    .menu-main::before{padding-right: 0;}
    .menu-mai{padding-left:0}
    .left,
    .right{width: auto;}
  }
</style>

<script>
  export default {
    name: 'LayoutsDemosBaselineFlipped',
    props: {
      source: String,
    },
    data: () => ({
      str_data: '',
      page: 1,
      pageCount: 0,
      itemsPerPage: 5,
      singleSelect: false,
      selected: [],
      drawer: false,
      dialog: false,
      headers: [
        { text: 'Usuário',align: 'left',value: 'name',sortable: false},
        { text: 'Email', value: 'calories', sortable: false },
        { text: 'Data de inclusão', value: 'fat', align: 'center', sortable: false },
        { text: 'Data de alteração', value: 'carbs',  align: 'center', sortable: false},
        { text: 'Regras', value: 'protein', align: 'center', sortable: false},
        { text: 'Status', value: 'status', align: 'center', sortable: false, },
        { text: '', value: 'actions', align: 'center', sortable: false},
        { text: 'Ações', value: 'txt', sortable: false}
      ],
      desserts: [],
      editedIndex: -1,
    }),
    created () {
      this.initialize(),
      this.fetchUsers()
      this.currentlyDate()
    },
    methods: {
      currentlyDate: function(){
        var data  = new Date();
        var week  = new Array('Domingo','Segunda','Terça','Quarta','Quinta','Sexta','Sábado');
        var day   = data.getDate();
        var mounth= new Array('janeiro','fevereiro','março','abril','junho','julho','agosto', 'setembro', 'outubro', 'novembro', 'dezembro');
        var year  = data.getFullYear();
        var str_date;
        str_date = week[data.getDay()]+', '+ day + ' de ' + mounth[data.getDay()] + ' de ' + year;
        return str_date;
      },
      fetchUsers: function () {
        const baseURI = './data-table.json';
        this.$http.get(baseURI)
        .then((result) => {  
          this.desserts = result.data
        })
      },
      getColor (status) {
        if (status == 'ATIVO') return 'active'
        else if (status == 'INATIVO') return 'inactive'
        //else return 'green' 
      },
      initialize () {
        this.desserts = []     
      },
      deleteItem (item) {
        const index = this.desserts.indexOf(item)
        confirm('Tem certeza que quer excluir este item?') && this.desserts.splice(index, 1)
      },
    },
  }
</script>