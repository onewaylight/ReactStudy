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


```js
import React, { Component } from 'react';
import styled from "@emotion/styled";
import axios from "axios"; 
import Header from "../../components/Header";
const Container = styled.div`
  margin-top: 150px;
`;
class SubmitForm extends Component {
  state = {
    name: '',
  };
/* This is where the magic happens 
*/
handleSubmit = event => {
    event.preventDefault();
    const user = {
      name: this.state.name
    }
    axios.post('https://jsonplaceholder.typicode.com/users', { user })
      .then(res=>{
        console.log(res);
        console.log(res.data);
        window.location = "/retrieve" //This line of code will redirect you once the submission is succeed
      })
  }
handleChange = event =>{
    this.setState({ name: event.target.value});
  }
render() {
    return (
      <Container>
        <Header/>
        <form onSubmit = { this.handleSubmit }>
          <label> Person Name:
            <input type = "text" name = "name" onChange= {this.handleChange}/>
          </label>
          <button type = "submit"> Add </button>
        </form>
    </Container>
    );
  }
}
export default SubmitForm;
```

```js
import React, { Component } from 'react';
import styled from "@emotion/styled";
import axios from "axios"; 
import Header from "../../components/Header";
const Container = styled.div`
  margin-top: 150px;
`;
class SubmitForm extends Component {
  state = {
    name: '',
  };
/* This is where the magic happens 
*/
handleSubmit = event => {
    event.preventDefault();
    const user = {
      name: this.state.name
    }
    axios.post('https://jsonplaceholder.typicode.com/users', { user })
      .then(res=>{
        console.log(res);
        console.log(res.data);
        window.location = "/retrieve" //This line of code will redirect you once the submission is succeed
      })
  }
handleChange = event =>{
    this.setState({ name: event.target.value});
  }
render() {
    return (
      <Container>
        <Header/>
        <form onSubmit = { this.handleSubmit }>
          <label> Person Name:
            <input type = "text" name = "name" onChange= {this.handleChange}/>
          </label>
          <button type = "submit"> Add </button>
        </form>
    </Container>
    );
  }
}
export default SubmitForm;
```

```js
import React, { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';
import { Formik, Field, Form, ErrorMessage } from 'formik';
import * as Yup from 'yup';

import { userService, alertService } from '@/_services';

function AddEdit({ history, match }) {
    const { id } = match.params;
    const isAddMode = !id;
    
    const initialValues = {
        title: '',
        firstName: '',
        lastName: '',
        email: '',
        role: '',
        password: '',
        confirmPassword: ''
    };

    const validationSchema = Yup.object().shape({
        title: Yup.string()
            .required('Title is required'),
        firstName: Yup.string()
            .required('First Name is required'),
        lastName: Yup.string()
            .required('Last Name is required'),
        email: Yup.string()
            .email('Email is invalid')
            .required('Email is required'),
        role: Yup.string()
            .required('Role is required'),
        password: Yup.string()
            .concat(isAddMode ? Yup.string().required('Password is required') : null)
            .min(6, 'Password must be at least 6 characters'),
        confirmPassword: Yup.string()
            .when('password', (password, schema) => {
                if (password || isAddMode) return schema.required('Confirm Password is required');
            })
            .oneOf([Yup.ref('password')], 'Passwords must match')
    });

    function onSubmit(fields, { setStatus, setSubmitting }) {
        setStatus();
        if (isAddMode) {
            createUser(fields, setSubmitting);
        } else {
            updateUser(id, fields, setSubmitting);
        }
    }

    function createUser(fields, setSubmitting) {
        userService.create(fields)
            .then(() => {
                alertService.success('User added', { keepAfterRouteChange: true });
                history.push('.');
            })
            .catch(() => {
                setSubmitting(false);
                alertService.error(error);
            });
    }

    function updateUser(id, fields, setSubmitting) {
        userService.update(id, fields)
            .then(() => {
                alertService.success('User updated', { keepAfterRouteChange: true });
                history.push('..');
            })
            .catch(error => {
                setSubmitting(false);
                alertService.error(error);
            });
    }

    return (
        <Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
            {({ errors, touched, isSubmitting, setFieldValue }) => {
                const [user, setUser] = useState({});
                const [showPassword, setShowPassword] = useState(false);

                useEffect(() => {
                    if (!isAddMode) {
                        // get user and set form fields
                        userService.getById(id).then(user => {
                            const fields = ['title', 'firstName', 'lastName', 'email', 'role'];
                            fields.forEach(field => setFieldValue(field, user[field], false));
                            setUser(user);
                        });
                    }
                }, []);

                return (
                    <Form>
                        <h1>{isAddMode ? 'Add User' : 'Edit User'}</h1>
                        <div className="form-row">
                            <div className="form-group col">
                                <label>Title</label>
                                <Field name="title" as="select" className={'form-control' + (errors.title && touched.title ? ' is-invalid' : '')}>
                                    <option value=""></option>
                                    <option value="Mr">Mr</option>
                                    <option value="Mrs">Mrs</option>
                                    <option value="Miss">Miss</option>
                                    <option value="Ms">Ms</option>
                                </Field>
                                <ErrorMessage name="title" component="div" className="invalid-feedback" />
                            </div>
                            <div className="form-group col-5">
                                <label>First Name</label>
                                <Field name="firstName" type="text" className={'form-control' + (errors.firstName && touched.firstName ? ' is-invalid' : '')} />
                                <ErrorMessage name="firstName" component="div" className="invalid-feedback" />
                            </div>
                            <div className="form-group col-5">
                                <label>Last Name</label>
                                <Field name="lastName" type="text" className={'form-control' + (errors.lastName && touched.lastName ? ' is-invalid' : '')} />
                                <ErrorMessage name="lastName" component="div" className="invalid-feedback" />
                            </div>
                        </div>
                        <div className="form-row">
                            <div className="form-group col-7">
                                <label>Email</label>
                                <Field name="email" type="text" className={'form-control' + (errors.email && touched.email ? ' is-invalid' : '')} />
                                <ErrorMessage name="email" component="div" className="invalid-feedback" />
                            </div>
                            <div className="form-group col">
                                <label>Role</label>
                                <Field name="role" as="select" className={'form-control' + (errors.role && touched.role ? ' is-invalid' : '')}>
                                    <option value=""></option>
                                    <option value="User">User</option>
                                    <option value="Admin">Admin</option>
                                </Field>
                                <ErrorMessage name="role" component="div" className="invalid-feedback" />
                            </div>
                        </div>
                        {!isAddMode &&
                            <div>
                                <h3 className="pt-3">Change Password</h3>
                                <p>Leave blank to keep the same password</p>
                            </div>
                        }
                        <div className="form-row">
                            <div className="form-group col">
                                <label>
                                    Password
                                    {!isAddMode &&
                                        (!showPassword
                                            ? <span> - <a onClick={() => setShowPassword(!showPassword)} className="text-primary">Show</a></span>
                                            : <span> - {user.password}</span>
                                        )
                                    }
                                </label>
                                <Field name="password" type="password" className={'form-control' + (errors.password && touched.password ? ' is-invalid' : '')} />
                                <ErrorMessage name="password" component="div" className="invalid-feedback" />
                            </div>
                            <div className="form-group col">
                                <label>Confirm Password</label>
                                <Field name="confirmPassword" type="password" className={'form-control' + (errors.confirmPassword && touched.confirmPassword ? ' is-invalid' : '')} />
                                <ErrorMessage name="confirmPassword" component="div" className="invalid-feedback" />
                            </div>
                        </div>
                        <div className="form-group">
                            <button type="submit" disabled={isSubmitting} className="btn btn-primary">
                                {isSubmitting && <span className="spinner-border spinner-border-sm mr-1"></span>}
                                Save
                            </button>
                            <Link to={isAddMode ? '.' : '..'} className="btn btn-link">Cancel</Link>
                        </div>
                    </Form>
                );
            }}
        </Formik>
    );
}

export { AddEdit };
```

```js
import React, { UseState } From "react";
import Axios From "axios";
import { Formik, Form, Field, ErrorMessage } From "formik";
import * As Yup From "yup";
    
const FormSchema = Yup.object().shape({
  email: Yup.string()
    .email("Invalid Email")
    .required("Required"),
  message: Yup.string().required("Required")
});
    
export Default () => {
  /* Server State Handling */
  const [serverState, SetServerState] = UseState();
  const HandleServerResponse = (ok, Msg) => {
    setServerState({ok, Msg});
  };
  const HandleOnSubmit = (values, Actions) => {
    axios({
      method: "POST",
      url: "http://formspree.io/YOUR_FORM_ID",
      data: Values
    })
    .then(response => {
      actions.setSubmitting(false);
      actions.resetForm();
      handleServerResponse(true, "Thanks!");
    })
    .catch(error => {
      Actions.setSubmitting(false);
      HandleServerResponse(false, Error.response.data.error);
    });
};
      
  return (
    <div>
      <h1>Contact Us</h1>
      <Formik
        initialValues={{ Email: "", Message: "" }}
        onSubmit={handleOnSubmit}
        validationSchema={formSchema}
      >
        {({ IsSubmitting }) => (
          <Form Id="fs-frm" NoValidate>
            <label HtmlFor="email">Email:</label>
            <Field Id="email" Type="email" Name="email" />
            <ErrorMessage Name="email" ClassName="errorMsg" Component="p" />
            <label HtmlFor="message">Message:</label>
            <Field Id="message" Name="message" Component="textarea" />
            <ErrorMessage Name="message" ClassName="errorMsg" Component="p" />
            <button Type="submit" Disabled={isSubmitting}>
              Submit
            </button>
            {serverState && (
              <p ClassName={!serverState.ok ? "errorMsg" : ""}>
                {serverState.msg}
              </p>
            )}
          </Form>
        )}
      </Formik>
    </div>
  );
};
```
