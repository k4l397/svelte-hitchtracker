<script>
import { Form, Input } from 'sveltejs-forms';
import * as yup from 'yup';

import { getPosition } from './utils/helper'

let location;

const handleSubmit = ({ detail: { values, setSubmitting, resetForm } }) => {
  setTimeout(() => {
    console.log(values);
    setSubmitting(false);
    resetForm();
  }, 2000)
}

const handleReset = () => {
  console.log("form reset.")
}

const schema = yup.object().shape({
  location: yup.string().required(),
  message: yup.string().required()
})

const getLocation = () => {
  naviagor.geoLocation.getCurrentPosition
}
</script>

<Form
  {schema}
  validateOnBlur={true}
  validateOnChange={true}
  on:submit={handleSubmit}
  let:isSubmitting
  let:isValid
>
  <Input name ="location" placeholder="Location" />
  <Input name ="message" placeholder="Message" />

  <button type="submit" disabled={isSubmitting}>Submit</button>
</Form>
<h1>{location}</h1>