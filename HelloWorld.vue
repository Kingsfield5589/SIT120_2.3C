<template>
  <div>
    <!-- 1. Template Syntax -->
    <h2>Template Syntax</h2>
    <p>{{ message }}</p>
    <p v-html="rawHTML"></p>
    <button v-bind:id="buttonId">Click me</button>
    <p>{{ message + ' ' + greet() }}</p>

    <!-- 2. Methods -->
    <h2>Methods</h2>
    <button @click="sayHello">Say Hello</button>
    <button @click="incrementCount">Increment Count (Method Handler)</button>

    <!-- 3. Reactivity Fundamentals -->
    <h2>Reactivity Fundamentals</h2>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment (Inline Handler)</button>

    <!-- 4. Computed Properties -->
    <h2>Computed Properties</h2>
    <p>Full Name: {{ fullName }}</p>

    <!-- 5. Class and Style Bindings -->
    <h2>Class and Style Bindings</h2>
    <p v-bind:class="{ active: isActive, 'text-danger': isError }">Styled Text</p>
    <p v-bind:style="dynamicStyles">Styled Text</p>

    <!-- 6. v-model with form elements and modifiers -->
    <h2>v-model with form elements and modifiers</h2>
    
    <!-- Text Input with .trim modifier -->
    <div>
      <label for="textInputTrim">Text Input (with .trim modifier): </label>
      <input type="text" id="textInputTrim" v-model.trim="textInputValue" />
      <p>Text Input Value (trimmed): {{ textInputValue }}</p>
    </div>

    <!-- Text Input with .number modifier -->
    <div>
      <label for="textInputNumber">Number Input (with .number modifier): </label>
      <input type="text" id="textInputNumber" v-model.number="numberValue" />
      <p>Number Input Value (converted to number): {{ numberValue }}</p>
    </div>

    <!-- Text Input with .lazy modifier -->
    <div>
      <label for="textInputLazy">Text Input (with .lazy modifier): </label>
      <input type="text" id="textInputLazy" v-model.lazy="lazyValue" />
      <p>Text Input Value (updated on blur): {{ lazyValue }}</p>
    </div>

    <!-- 7. Watchers -->
    <h2>Watchers</h2>
    
    <!-- Watcher for textInputValue -->
    <div>
      <label for="textInputWatcher">Watch textInputValue: </label>
      <input type="text" id="textInputWatcher" v-model="textInputValue" />
      <p>Text Input Value: {{ textInputValue }}</p>
      <p>Length of Text Input Value: {{ textInputLength }}</p>
    </div>

    <!-- Watcher for checkboxValue -->
    <div>
      <label for="checkboxWatcher">Watch checkboxValue: </label>
      <input type="checkbox" id="checkboxWatcher" v-model="checkboxValue" />
      <p>Checkbox Value: {{ checkboxValue }}</p>
      <p>Checkbox State: {{ checkboxState }}</p>
    </div>

    <!-- 8. Components, Props, Events, and Slots -->
    <h2>Components, Props, Events, and Slots</h2>

    <!-- ChildComponent with Props and Event -->
    <child-component :message="messageFromParent" @child-event="handleChildEvent">
      <template v-slot:default="{ messageFromSlot }">
        <p>Message from Slot: {{ messageFromSlot }}</p>
      </template>
    </child-component>
  </div>

    <!-- Child Component Definition -->
    <div>
      <h3>Child Component</h3>
      <p>Message from Parent: {{ message }}</p>
      <button @click="emitEvent">Emit Event to Parent</button>
      <slot :messageFromSlot="message"></slot>
    </div>

</template>

<script setup>
import { ref, computed, watch } from 'vue';

// 1. Template Syntax
const message = 'Hello';
const rawHTML = '<span style="color: red;">This is raw HTML</span>';
const buttonId = 'myButton';

// 2. Methods
const sayHello = () => {
  alert('Hello, Vue 3!');
};

const incrementCount = () => {
  count.value++;
};

// 3. Reactivity Fundamentals
const count = ref(0);

// 4. Computed Properties
const firstName = ref('John');
const lastName = ref('Doe');
const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`;
});

// 5. Class and Style Bindings
const isActive = ref(true);
const isError = ref(false);
const dynamicStyles = computed(() => {
  return {
    color: isActive.value ? 'blue' : 'black',
    fontSize: isError.value ? '18px' : '14px',
  };
});

// 6. v-model with form elements and modifiers
const textInputValue = ref('');
const numberValue = ref(0);
const lazyValue = ref('');

// 7. Watchers
const textInputLength = ref(0);
watch(textInputValue, (newValue) => {
  textInputLength.value = newValue.length;
});

const checkboxValue = ref(false);
const checkboxState = ref('Unchecked');
watch(checkboxValue, (newValue) => {
  if (newValue) {
    checkboxState.value = 'Checked';
  } else {
    checkboxState.value = 'Unchecked';
  }
});

// 8. Components, Props, Events, and Slots
const messageFromParent = ref('Hello from Parent');
const handleChildEvent = (message) => {
  alert(`Received from Child: ${message}`);
};

 
</script>


<script setup props="props, { emit }">
const message = props.message;

const emitEvent = () => {
  emit('child-event', 'Hello from Child');
};
</script>
