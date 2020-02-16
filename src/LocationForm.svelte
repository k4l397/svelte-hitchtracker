<script>
import * as yup from 'yup';
import { getPosition } from './utils/helper'

const formValues = {
  location: '',
  message: ''
};

let currentError = -1;
let errors = null;
  
const schema = yup.object().shape({
    location: yup.string().required(),
    message: yup.string().required()
  });

const handleSubmit = async () => {
  schema.validate(formValues, {abortEarly: false})
    .then((valid) => {
      errors = null;
      console.log(valid);
    })
    .catch((err) => {
      errors = err;
      console.log(err)
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
  margin: 0.5em 0 0 0
}

p {
  margin: 0
}

button {
  margin-top: 0.5em
}
</style>

<form autocomplete="off" on:submit|preventDefault={handleSubmit}>
  <input  name="location" type="text" bind:value={formValues.location} placeholder="Location" />
  {#if errors !== null &&
    -1 !== (currentError =
    errors.inner.reduce((errorIndex, err, currentIndex) => {return err.path === "location" ? currentIndex : errorIndex}, -1 ))}
    <p>{errors.inner[currentError].message}</p>
  {/if}
  <input  name="message" type="text" bind:value={formValues.message} placeholder="Message" />
  {#if errors !== null &&
    -1 !== (currentError =
    errors.inner.reduce((errorIndex, err, currentIndex) => {return err.path === "message" ? currentIndex : errorIndex}, -1 ))}
    <p>{errors.inner[currentError].message}</p>
  {/if}
  <button type="submit">Send Update</button>
</form>