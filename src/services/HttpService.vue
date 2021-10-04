<script>
import axios from "axios";

const BASE_URL = "http://localhost:3000/api/v1";
const TIMEOUT = 1000;

class HttpService {
  constructor() {
    let service = axios.create({
      baseURL: BASE_URL,
      timeout: TIMEOUT,
      // headers: {
      //   Authorization: `Bearer ${window.sessionStorage.getItem("token")}`,
      // },
    });

    service.interceptors.response.use(this.handleSuccess, this.handleError);

    this.service = service;
  }

  handleSuccess(response) {
    return response;
  }

  handleError = (error) => {
    const jsonError = error.toJSON();
    switch (error.response.status) {
      case 400:
        console.log(jsonError);
        break;
      case 401:
        console.log(jsonError);
        break;
      case 404:
        console.log(jsonError);
        break;
      default:
        console.log(jsonError);
        break;
    }
    return Promise.reject(error);
  };

  get(path, payload, callback) {
    return this.service.get(path, payload).then((response) => callback(response.status, response.data));
  }

  // patch(path, payload, callback) {
  //   return this.service
  //     .request({
  //       method: "PATCH",
  //       url: path,
  //       responseType: "json",
  //       data: payload,
  //     })
  //     .then((response) => callback(response.status, response.data));
  // }

  // post(path, payload, callback) {
  //   return this.service
  //     .request({
  //       method: "POST",
  //       url: path,
  //       responseType: "json",
  //       data: payload,
  //     })
  //     .then((response) => callback(response.status, response.data));
  // }
}
export default new HttpService();
</script>
