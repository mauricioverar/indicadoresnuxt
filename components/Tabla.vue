<template>
  <div>
    <h1>tabla</h1>
    <div>
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th v-for="(item, clave) in datos" :key="clave">
                {{ item.fecha }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td v-for="(item, clave) in datos" :key="clave">
                {{ item.valor }}
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tabla",

  props: ["datos"],
};
</script>
