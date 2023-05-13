#### Design Pattern Notes
[Pattern.dev](https://www.patterns.dev/)
***
[Medium_ReactPattern](https://medium.com/javascript-in-plain-english/5-react-design-patterns-you-should-know-629030e2e2c7)
***
[Design Pattern](https://medium.com/@obrm770/best-practices-and-design-patterns-in-react-js-for-high-quality-applications-6b203be747fb)
#### Authentication with xAuthToken
```js
import axios from "axios";
import { message } from "antd";
import Config from "./config";
import { getAppLanguages } from "./util";

const HttpService = axios.create({
  baseURL: Config.baseUrl + Config.prefix,
  headers: {
    "X-Auth-Token":
      window.localStorage.getItem("xAuthToken") !== undefined
        ? `${window.localStorage.getItem("xAuthToken")}`
        : window.sessionStorage.getItem("preAuthToken") !== undefined
        ? `${window.sessionStorage.getItem("preAuthToken")}`
        : "",
    "Cross-Domain": "true",
    Accept: "*/*",
  },
});

const logoutMechanism = () => {
  sessionStorage.clear();
  localStorage.clear();
  window.location.pathname = "/login";
};

HttpService.interceptors.request.use(
  (config) => {
    const { headers } = config;

    if (headers) {
      const auth = headers["X-Auth-Token"];

      if (
        headers &&
        (window.localStorage.getItem("xAuthToken") ||
          window.sessionStorage.getItem("preAuthToken"))
      ) {
        headers["X-Auth-Token"] =
          (window.localStorage.getItem("xAuthToken") as string) ||
          (window.sessionStorage.getItem("preAuthToken") as string);
      }

      if (
        !window.localStorage.getItem("xAuthToken") &&
        !window.sessionStorage.getItem("preAuthToken")
      ) {
        delete headers["X-Auth-Token"];
      }

      if (headers) {
        headers["Accept-Language"] =
          getAppLanguages() === "fa" ? "fa-IR" : "en-US";
      }

      if (!auth || (auth && auth === "Bearer null")) {
        return Promise.reject(config);
      }
    }

    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

HttpService.interceptors.response.use(
  (response) => {
    return response;
  },
  async (error) => {
    if (!error || !error.response) {
      return Promise.reject(error);
    }

    const originalRequest = error.config;

    if (error.response.status === 403) {
      return Promise.reject(error);
    }

    if (error.response.status === 401) {
      const ref_token = localStorage.getItem("refresh_token");
      const token_request = sessionStorage.getItem("token_request");

      if (!ref_token && !token_request) {
        logoutMechanism();
        return Promise.reject(error);
      }

      if (!token_request) {
        const timer = setTimeout(() => {
          sessionStorage.setItem("token_request", "1");
        }, 2000);
        const params = new URLSearchParams();
        params.append("grant_type", "refresh_token");
        if (ref_token != null) {
          params.append("refresh_token", ref_token);
        }
        if (!ref_token) {
          logoutMechanism();
        }
      }
    }

    // 500
    if (error.response.status === 500) {
      const lastError = localStorage.getItem("500");
      const t = new Date().getTime();

      if (lastError) {
        if (t - Number(lastError) > 60000) {
          localStorage.setItem("500", t.toString());
          message.error("Error in network");
        }
      } else {
        localStorage.setItem("500", t.toString());
        message.error("Error in network");
      }
      return Promise.reject(error);
    }

    if (
      error.response.data &&
      (error.response.data.errors || error.response.data.message)
    ) {
      const { errors, message: messageServer } = error.response.data;

      // errors
      if (errors && errors.length > 0) {
        errors.forEach((error: { field: string; message: string }) => {
          const { field, message: eMassage } = error;
          message.error(`${field}: ${eMassage}`);
        });
      }
      if (messageServer && messageServer !== "Bid not found.") {
        message.error(messageServer);
      }

      return Promise.reject(error);
    }
    return Promise.reject(error);
  }
);

export default HttpService;

```

#### Generic react component in typescript
[Generic Component](https://medium.com/edonec/creating-a-generic-component-with-react-typescript-2c17f8c4386e#:~:text=Definition,and%20use%20their%20own%20types.)



