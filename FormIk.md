### 

```ts
  return (
    <Formik
      initialValues={getInitialValues(event, range)}
      validationSchema={Yup.object().shape({
        allDay: Yup.bool(),
        description: Yup.string().max(5000),
        end: Yup.date().when(
          'start',
          (start: Date, schema: any) =>
            start &&
            schema.min(start, t('The end date should be after start date'))
        ),
        start: Yup.date(),
        title: Yup.string().max(255).required(t('The title field is required'))
      })}
      onSubmit={async (
        values,
        { resetForm, setErrors, setStatus, setSubmitting }
      ) => {
        try {
          const data = {
            allDay: values.allDay,
            description: values.description,
            end: values.end,
            start: values.start,
            title: values.title
          };

          if (event) {
            dispatch(updateEvent(event.id, data));
          } else {
            dispatch(createEvent(data));
          }

          resetForm();
          setStatus({ success: true });
          setSubmitting(false);
          enqueueSnackbar(t('The calendar has been successfully updated!'), {
            variant: 'success',
            anchorOrigin: {
              vertical: 'bottom',
              horizontal: 'center'
            },
            TransitionComponent: Zoom
          });

          if (isCreating) {
            onAddComplete();
          } else {
            onEditComplete();
          }
        } catch (err) {
          console.error(err);
          setStatus({ success: false });
          setErrors({ submit: err.message });
          setSubmitting(false);
        }
      }}
    >
      {({
        errors,
        handleBlur,
        handleChange,
        handleSubmit,
        isSubmitting,
        setFieldValue,
        touched,
        values
      }) => (
        <form onSubmit={handleSubmit}>
          <Box p={3}>
```

```js
<Formik
  initialValues={initialValues}
  onSubmit={(values) => {
    console.log(values) // Object holds your form values
    axios({
      method: "post",
      url: "url",
      data: { values }
    })
  })
/>
```

```ts
import * as React from "react"
import { render } from "react-dom"
import { Formik, Form, Field, FormikHelpers as FormikActions } from "formik"
import axios from "axios"
import "./skeleton.css"
import "./main.css"

interface Values {
  firstName: string
  lastName: string
  email: string
}

const BasicForm: React.FC<{}> = () => {
  const initialValues: Values = { firstName: "", lastName: "", email: "" }

  return (
    <div className=" container">
      <h4>Formik x TypeScript</h4>

      <Formik
        initialValues={initialValues}
        onSubmit={(
          values: Values,
          { setStatus, resetForm }: FormikActions<Values>
        ) => {
          axios({
            method: "post",
            url: "https://getform.io/f/486e12c3-81c9-435e-b18a-498756d0002e",
            data: { values }
          })
            .then(res => {
              setStatus(res.status)
              if (res.status === 200) {
                resetForm()
                setStatus({
                  sent: true,
                  msg: "Message has been sent! Thanks!"
                })
              }
              // Set other status if you like
            })
            .catch(err => {
              resetForm()
              setStatus({
                sent: false,
                msg: `Error! ${err}. Please try again later.`
              })
            })
        }}
      >
        {({ isSubmitting, status }) => (
          <Form>
            <label htmlFor="firstName">First Name</label>
            <Field
              id="firstName"
              name="firstName"
              placeholder="John"
              type="text"
            />

            <label htmlFor="lastName">Last Name</label>
            <Field
              id="lastName"
              name="lastName"
              placeholder="Doe"
              type="text"
            />

            <label htmlFor="email">Email</label>
            <Field
              id="email"
              name="email"
              placeholder="john@acme.com"
              type="email"
            />

            {status && status.msg && (
              <p
                className={`alert ${
                  status.sent ? "alert-success" : "alert-error"
                }`}
              >
                {status.msg}
              </p>
            )}
            <button
              type="submit"
              style={{
                display: "block"
              }}
              disabled={isSubmitting}
            >
              Submit {isSubmitting && <span>Sending...</span>}
            </button>
          </Form>
        )}
      </Formik>
    </div>
  )
}

render(<BasicForm />, document.getElementById("root"))
```
