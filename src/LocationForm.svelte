<script>
import * as yup from 'yup';
import { getPosition } from './utils/helper'

const formValues = {
  location: {value: '', error: ''},
  message: {value: '', error: ''}
};

let currentError = -1;
let errors = false;
  
const schema = yup.object().shape({
    location: yup.object().shape({value: yup.string().required()}),
    message: yup.object().shape({value: yup.string().required()}),
});

const handleSubmit = async () => {
  schema.validate(formValues, {abortEarly: false})
    .then((valid) => {
      errors = false;
      Object.keys(valid).forEach((key) => formValues[key].error = '');
      console.log(valid);
    })
    .catch((err) => {
      errors = true;
      console.log(err);
      if (err.inner){
        err.inner.forEach((innerErr) => {
          console.log(innerErr.path)
          formValues[innerErr.path.substr(0, innerErr.path.indexOf('.'))].error = innerErr.message
        })
      }
      else {
        formValues[err.path.substr(0, innerErr.path.indexOf('.'))].error = err.message
      }
    })
};

const handleReset = () => {
  console.log('form reset.')
};


const getLocation = () => {
  naviagor.geoLocation.getCurrentPosition
};
</script>

<style>
form {
  display: flex;
  justify-content: center;
  flex-direction: column;
}

input {
  margin: 0.5em 0 0 0;
}

p {
  margin: 0;
  color: red;
}

button {
  margin-top: 0.5em;
}
</style>

<form autocomplete="off" on:submit|preventDefault={handleSubmit}>
  <input  name="location" type="text" bind:value={formValues.location.value} placeholder="Location" />
    {#if errors === true && formValues.location.error !== ''}
      <p>Field is invalid.</p>
    {/if}
  <input  name="message" type="text" bind:value={formValues.message.value} placeholder="Message" />
    {#if errors === true && formValues.message.error !== ''}
      <p>Field is invalid.</p>
    {/if}
  <button type="submit">Send Update</button>
</form>