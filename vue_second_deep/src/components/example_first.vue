<template>
  <span>
    I'm Test component for Vue 2
  </span>
</template>

<script>
export default {
  name: 'ExampleFirst',
  data() {
    return {
      amount: 0,
    };
  },

  watch: {
    amount(newVal) {
      console.log(newVal);
    },
  },

  beforeCreate() {
    console.log('this.amount beforeCreate ', this.amount);
    this.amount = 1;
    },
  created() {
    console.log('this.amount created ', this.amount);
    this.amount = 2;
    },
  beforeMount() {
  console.log('this.amount beforeMount ', this.amount);
    this.amount = 3;
    },
  mounted() {
  console.log('this.amount mounted ', this.amount);
  console.log('this.$el mounted ', this.$el);
    this.amount = 4;
    },
  beforeDestroy() {
  console.log('this.amount beforeDestroy ', this.amount);
    console.log('this.$el beforeDestroy ', this.$el);
    this.amount = 5;
    },
  destroyed() {
  console.log('this.amount destroyed ', this.amount);
    console.log('this.$el destroyed ', this.$el);
    this.amount = 6;
    },
};

/*
Давай поподробнее

Во vue 3

beforeCreate - создается instance - он инициализировался.
Вызывается перед обработкой data, computed. Разумеется в этом методе не вызывается watcher

created - вызывается после обработки всех параметров связанных с состоянием и до монтирования компонента.
Здесь доступны свойства data computed watcher, а вот $el не доступен. То есть здесь watcher отработает.

beforeMount - вызывается непосредственно перед монтированием компонента. $el не доступен. То есть здесь watcher отработает.
DOM узлы (nodes) не созданы еще;

mounted - вызывается после монтирования. $el доступен. То есть здесь watcher отработает.

Пара моментов этого метода жизненного цикла
 а) он не вызывается при sever-side rendering
 б) что значит смонтировался? Это означает что все дочерние компоненты вмонтированы в DOM
 и он сам компонент вмонтирован в родительское дерево
 в) Все компоненты кроме тех кто с Suspense

beforeUpdate - вызывается перед обновлением DOM дерева, вызванным изменением
реактивного состояния комапонента.

В нем можно безопасно менять состояние компонента + и в нем можно пдотянутся до DOM и до состояния компонента
которые прежние и еще до обновления.

он не вызывается при sever-side rendering

updated - вызывается после того как дерево DOM обновилось из-за обновления реактивного состояния компонента.
То есть да - этот метод жизненго цикла вызывется у родителя только после того как метод жизненного цикла updated вызвался у ребенка.
В нем не стоит менять состояние компонента - это может привести к вечному замкнутому циклу

он не вызывается при sever-side rendering

А если нужно не после всех изменений а только посдле конкретного? тогда nextTick() - но его то юзают при немного интересных кейсах

beforeUnmount - вызывается перед размонтированием компонента
instance компонента все еще доступен $el тоже;
он не вызывается при sever-side rendering

unmounted - вызывается после размонтирования компонента
instance компонента все еще доступен $el тоже;
Что значит размонтирован?

а) размонтирован из DOM и все его дочерние тоже
б) остановлены watcher computed и все side effects.
в) Здесь согласно доке надо очищать таймауты, удалять обработчик события и отписываться от него,
и вообще очичтить и отписаться от все сайд эффектов своих. Это разнится с докой Vue2у которой
это все рекомендовала дока делать в beforeDestroy.

Vue отпишется от наблюдения за изменением переменной перед вызовом хуков beforeDestroy и destroyed, поэтому 5 и 6 не попадут в консоль.

3 и 4  выводится во vue3 и только 4 выводится во vue2. Почему?

Так как все эти хуки вызываются во время одного тика, Vue вызовет watcher один раз в самом конце, со значением 4.

* */
</script>